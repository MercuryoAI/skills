# MagicPay Command Guide

The hard rules from `SKILL.md` apply to every command: protect the
MagicPay API key and CDP endpoint, use only the browser/session approved
for this task, keep protected form resolution to fill-only, and
get the matching typed MagicPay approval before any submit, protected action,
purchase, login, identity submission, account change, or other consequential
action.

## Setup And Readiness

### `magicpay init <apiKey> [--api-url <url>]`

Save the API key to `~/.magicpay/config.json`. When `--api-url` is provided,
`init` also stores the gateway base URL there. Omit `--api-url` for normal
setup; the CLI uses its bundled default MagicPay gateway URL. Pass
`--api-url <url>` only for a non-default staging, self-hosted, or test gateway.

Do not print, log, or share the API key or the persisted config. If this
machine or workspace is shared or compromised, ask the user to rotate or
revoke the key before continuing.

### `magicpay status`

Check CLI health, authenticated identity, and update state. Use this as the
normal preflight command before a protected-form task.

### `magicpay doctor`

Inspect the local config file when `status` still fails after `init`.

### `magicpay --version`

Print the installed CLI version.

## Product Session And Browser Child Control

### `magicpay start-session [name] [--merchant-name <name>]`

Start the MagicPay product workflow session. This is the parent operation for
normal MagicPay product work; it creates the product workflow before any
browser child is required.

`start-session` attempts to cancel/clear a stale previous workflow binding
before it creates the new product session. If that recovery is still blocked,
start manual recovery with `magicpay status`, then either `magicpay
end-session` or a fresh `start-session`.

### `magicpay launch [url] [--profile <name>]`

Launch a browser child inside the active MagicPay product workflow session.

Use this after `magicpay start-session` when MagicPay should create the
browser execution resource. The optional URL places the new browser child at
the starting page. The browser child does not replace the product workflow
identity.

### `magicpay attach <cdp-url> [--provider <name>]`

Attach an already running browser as the browser child inside the active
MagicPay product workflow session.

Use only a private CDP endpoint for the browser/session the user approved for
this task. Treat the endpoint as sensitive because it inherits the browser's
logged-in state. Run `attach` after `start-session` when MagicPay is not yet
bound to the approved browser child, or when the CDP endpoint changed.
Re-attaching the same endpoint is allowed but is not required as a ritual.

### `magicpay browser-status`

Inspect the browser child bound to the active MagicPay product workflow.

This is a browser-dependent diagnostic command. Browser-only state is not
enough; the command requires an active product workflow and a matching browser
child binding.

### `magicpay close`

Close or clear the browser child bound to the active MagicPay product
workflow.

This does not end the product workflow session. Use it when the browser child
should be cleaned up or replaced, then continue the same product workflow with
another `launch` or `attach` if needed.

### `magicpay solve-captcha [--timeout <s>]`

Solve a confirmed CAPTCHA on the current browser child inside the active
MagicPay product workflow.

Only call this when a real CAPTCHA is confirmed present. The command uses the
current bound browser child, and does not close or recreate the browser. After
a successful solve, continue the ordinary browser or protected-form flow from
the current page. If the next step is through MagicBrowse, call
`magicbrowse mark-captcha-resolved`, then continue with `magicbrowse act
"continue..."`.

### `magicpay end-session`

Complete the active product workflow session and product root run.

This is workflow completion only. After it succeeds, return control to the
browser owner. A browser tool or orchestrator that launched an owned
disposable browser may clean up its own session when the overall task is done;
an external/user-owned browser stays open unless the user explicitly approves
teardown. `end-session` does not require a live browser child.

## Protected-Form Flow

### `magicpay find-form [--purpose <auto|login|identity|payment_card>]`

Discover the supported protected form on the current page and return the
current protected-form contract.

### `magicpay resolve-form <fillRef> [--item-ref <vaultItemId>] [--refresh-fields <field1,field2>]`

Resolve one protected form target through MagicPay. The CLI creates the
request, waits for the result, and fills the target. It does not submit the
form. Use `--item-ref` to pin one vault item instead of letting MagicPay choose
from the available candidates. Use `--refresh-fields` to ask MagicPay to
refresh selected stored fields while resolving the current form.

After a successful fill, continue the browser task through the browser owner.
If MagicBrowse produced a protected-form handoff, call `magicbrowse act` with
the returned `handoff.resumeObjective`.

### `magicpay resolve-fields <target-id...>`

Refresh the session-local open-data snapshot, then match one or more observed
non-secret target ids against that fresh snapshot (name, email, phone, locale,
date of birth, address, and similar reusable public facts). Returns `matched`,
`ambiguous`, or `no_match` per target. Targets already owned by the protected
lane stay excluded. The target ids come from the companion browser tool's
latest observation. In orchestration, auto-fill only `matched` results — never
invent values for `ambiguous` or `no_match`. If MagicPay cannot refresh open
data, the command fails closed instead of using stale profile facts.

Without `--request-missing`, the command only matches values already present in
the session/profile snapshot and never creates a user request. With
`--request-missing`, the command may create a MagicPay data request for the
explicit target ids whose field meaning is clear and whose value is missing.
Use it only for open fields required to advance the current user task. Do not
use it for optional newsletter, marketing, promo, survey, analytics, or similar
fields, even when they are visible on the same page.

On sensitive identity or payment pages, review matched profile autofills
before applying them. Use only target ids from the latest observation.

### `magicpay authorize-payment --amount <number> --currency <code> --recipient <name> [--description <text>] [--recurring <true|false>] [--authorization-ref <ref>] [--item-ref <vaultItemId>] [--return-pending]`

Request approval for a payment authorization through the structured
`authorize_payment` action contract.

Before calling it, collect these visible transaction facts from the current
checkout/review page and the user's task:

- `amount` — final amount in major units as a JSON number, not cents and not a
  formatted string.
- `currency` — explicit three-letter currency code.
- `recipient` — merchant or payee the user believes they are paying.
- `description` — optional short order, plan, subscription, or purpose summary.
- `recurring` — optional boolean; ask the user if recurring status matters and
  is unclear.

`--item-ref` remains the existing vault item selector path. It is not placed in
`params`, and this command does not change how MagicPay discovers or selects
vault items.

After successful approval, continue with that exact payment: protected payment
artifact use, payment form fill, and final Pay/Submit are covered while
`amount`, `currency`, `recipient`, and `recurring` stay unchanged. Stop and ask
again if any of those facts change.

Use `--return-pending` when the agent needs to hand approval to the user
without blocking the command. It creates the same pending request, stores
`currentRequestId`, and returns the request handle. The user can then approve
in MagicPay web/mobile UI or provide the OTP they received. OTP is optional.

### `magicpay sign-message --item-ref <walletItemId> --message <text> [--return-pending]`

Request approval to sign one exact wallet message with the selected wallet
item. Use this for wallet message signing only. After approval, sign exactly
that message; stop and ask again if the message changes.

Use `--return-pending` for the same non-blocking approval handoff described
above.

### `magicpay confirm-action --summary <text> [--details <text>] [--return-pending]`

Request approval for a non-payment consequential action that has no more
specific typed command. Use a concise summary that names the visible action;
add details when the page context, recipient, account, or consequences need to
be explicit.

Use this only for consequential actions without a dedicated typed MagicPay
command. Payments use `authorize-payment`; wallet message signing uses
`sign-message`.

Use `--return-pending` for the same non-blocking approval handoff described
above.

### `magicpay confirm-otp --otp <digits> [--session <id>] [--request <id>]`

Confirm the active pending runtime request by OTP. Use this only after a
pending approval request exists and only when the user provides the OTP for
that request. By default the command uses the active workflow session and
`currentRequestId`; `--session` and `--request` are recovery selectors.

Do not repeat the OTP in chat, summaries, logs, saved notes, or command
reports. If OTP is invalid, expired, or exhausted, report that typed failure
and keep MagicPay UI approval available while the request itself remains
pending.

### `magicpay wait-request [--session <id>] [--request <id>]`

Resume waiting for the active pending runtime request and claim its result
when it reaches a terminal result. Run this after either MagicPay UI approval
or OTP confirmation. The command clears `currentRequestId` only after a
terminal result or unrecoverable failure; timeout leaves the request resumable.
