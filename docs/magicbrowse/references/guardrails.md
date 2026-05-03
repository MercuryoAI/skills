# MagicBrowse Guardrails

The Hard Rules from SKILL.md, expanded to long form.

## Consequential Actions

`magicbrowse` can navigate, inspect, draft, and prepare. It must not
silently commit an account-affecting or irreversible action.

Stop and ask the user before:

- submitting a form;
- posting or sending content;
- accepting terms or confirming consent;
- changing account data, account settings, permissions, or privacy
  controls;
- booking, buying, ordering, subscribing, or paying;
- deleting, overwriting, publishing, or otherwise modifying remote
  data.

After approval, re-run `observe` so the target-id and visible state are
fresh, then execute only the exact final action the user approved. If
the page changed meaningfully, ask again rather than widening the
approval.

## MagicPay Boundary

The `magicbrowse` skill ends at the boundary of any protected form.
It gets the host *to* the form; it never *into* it.

**Forbidden field categories.** Do not use `act`, `type`, `fill`, or
`select` on:

- **Login / signup credentials.** Email, username, password, OTP,
  TOTP, magic-link inputs, "remember me" toggles tied to credential
  entry, social-auth connectors that solicit OAuth credentials in the
  same flow.
- **Identity-document fields.** Passport number, national ID number,
  KYC/AML address, date of birth tied to a verified identity,
  document expiry, document-issuing country, machine-readable-zone
  inputs, photo-of-document upload buttons.
- **Payment-card and banking fields.** Cardholder name when bound to
  the PAN, PAN, CVV/CVC, expiry, IBAN, BIC/SWIFT, sort code, routing
  number, account number, billing-address fields when they are part
  of the card form.
- **Vault- or secret-store-sourced values.** Any value whose origin is
  the user's vault, password manager, or other secret store, even if
  the field type itself looks generic.

The planner and navigator already refuse credential entry at the LLM
layer. This guardrail raises that refusal from a probabilistic LLM
behaviour to a host-facing contract: even if the planner *would*
refuse, the host must not attempt it. Switch to the `magicpay` skill
at the form boundary.

The narrow exception is **placeholder values to traverse a
non-protected screen** (e.g. typing dummy passenger names to reach
the payment page in a flight booking flow). Do not type real
identity data; use semantically obvious placeholders. The moment a
field starts asking for something protected, stop.

## Singleton Session

`$MAGICBROWSE_HOME/current-session.json` (default
`~/.magicbrowse/current-session.json`) is a singleton pointer. All
`magicbrowse` commands read and write it. Concurrent workflows on
the same home file silently overwrite each other's session state —
the second `launch` becomes the current session, the first one is
orphaned mid-task.

For multi-tenant or parallel use, set a distinct `MAGICBROWSE_HOME`
per workflow. Per-user tools that may run more than one `magicbrowse`
flow simultaneously must scope `MAGICBROWSE_HOME` per request, not
share the default.

This is not a security boundary — it is a correctness boundary.
Sharing `MAGICBROWSE_HOME` between concurrent workflows produces
silent cross-talk, not visible errors.

## Browser Authority

Use a fresh owned browser session by default. Existing CDP endpoints,
named profiles, and explicit `--user-data-dir` paths may already be
logged in to real accounts. Acting through them inherits that browser's
authority even though `magicbrowse` never receives the password.

Only use `magicbrowse attach`, `--profile`, or `--user-data-dir` when
the user explicitly approves that browser/session for the current
task. Keep CDP endpoints private and do not paste them into shared
logs. Close or detach when the workflow is done, and start a fresh
session for unrelated work.

## Page Context And Screenshots

LLM-backed `act` sends page state to the shared MagicPay gateway.
`act --use-vision` can include screenshots. Treat both as external
processing of the current page context.

Avoid private, sensitive, or unrelated pages unless the user approves
that workflow. Do not use vision mode on sensitive pages unless it is
explicitly required and approved. At protected forms, stop and switch
to `magicpay`.

## Captcha And Auth Walls

Both the planner and navigator are instructed to refuse to attempt
credential entry or solve captchas. When `act` runs into either, it
returns `status: completed` with a `finalMessage` describing the
required human action — *not* `status: failed`.

- **Do not** retry the same `act` after a captcha or auth-wall
  completion. The same prompt will hit the same wall.
- **Do not** script around captcha. There is no API for it in this
  skill, by design.
- **Do** surface the `finalMessage` to the user and let them decide
  whether to log in / solve the captcha and resume.

Once the user has cleared the wall, the orchestrator can issue the
next `act` on the same session — page state and planner memory
persist.

## Diagnostics

- `magicbrowse browser-status` inspects the live browser/page/runtime
  state. Use for debugging, not as a control-flow signal.
- `magicbrowse doctor` inspects the shared MagicPay gateway config.
  Use after `init` if `act` reports a missing-key error.
- `magicbrowse close` is teardown or recovery, never a success
  signal. Task success comes from the `act` `status` and
  `finalMessage`, not from the close.

## Ask The User When

- `doctor` fails and there is no `MAGICPAY_API_KEY` available;
- the environment cannot launch or attach to a Chrome session;
- the task requires `attach`, `--profile`, or `--user-data-dir`;
- `--use-vision` would expose screenshots of a private or sensitive
  page;
- `act` returns a `finalMessage` describing an auth wall, captcha, or
  any other state that requires the user's input;
- the next action would submit, post, send, save, delete, accept,
  book, buy, order, pay, publish, or otherwise commit a consequential
  change;
- the task crosses into a protected form — switch skill rather than
  improvising in `magicbrowse`.
