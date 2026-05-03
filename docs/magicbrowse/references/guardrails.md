# MagicBrowse Guardrails

The three Hard Rules from SKILL.md, expanded to long form.

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
- `act` returns a `finalMessage` describing an auth wall, captcha, or
  any other state that requires the user's input;
- the task crosses into a protected form — switch skill rather than
  improvising in `magicbrowse`.
