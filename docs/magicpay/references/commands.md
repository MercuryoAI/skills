# MagicPay Command Guide

The hard rules from `SKILL.md` apply to every command: protect the
MagicPay API key and CDP endpoint, use only the browser/session approved
for this task, default protected form resolution to fill-and-review, and
ask for explicit approval before any submit, protected action, purchase,
login, identity submission, account change, or other consequential action.

## Setup And Readiness

### `magicpay init <apiKey> [--api-url <url>]`

Save the API key to `~/.magicpay/config.json`. When `--api-url` is provided,
`init` also stores the gateway base URL there.

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

## Browser Attach And Session Control

### `magicpay attach <cdp-url> [--provider <name>]`

Connect MagicPay to an already running browser through CDP.

Use only a private CDP endpoint for the prepared browser/session the user
approved for this task. Treat the endpoint as sensitive because it inherits
the browser's logged-in state.

### `magicpay start-session [name] [--merchant-name <name>]`

Bind the attached browser to a MagicPay workflow session.

### `magicpay end-session`

Complete the active workflow session without closing the browser.

## Protected-Form Flow

### `magicpay find-form [--purpose <auto|login|identity|payment_card>]`

Discover the supported protected form on the current page and return the
current protected-form contract.

### `magicpay resolve-form <fillRef> [--item-ref <vaultItemId>] [--no-submit]`

Resolve one protected form target through MagicPay. The CLI creates the
request, waits for the result, and fills the target. Plain `resolve-form`
can also perform a guarded submit when that was explicitly approved and a safe
form-bound submit control is still live. Use `--item-ref` to pin one vault
item instead of letting MagicPay choose from the available candidates. Use
`--no-submit` when you want the filled result without guarded submit.

In orchestrated skill workflows, use `--no-submit` by default. Submit only
after the user explicitly confirms the current site/merchant, action, and
visible amount or data. Use plain `resolve-form <fillRef>` without
`--no-submit` only when that immediate guarded submit was already approved
for the current form.

### `magicpay resolve-fields <targetRef...> [--refresh-snapshot]`

Match one or more observed non-secret target refs against the session-local
open-data snapshot (name, email, phone, locale, date of birth, address, and
similar reusable public facts). Returns `matched`, `ambiguous`, or `no_match`
per target. Targets already owned by the protected lane stay excluded. The
target refs come from the companion browser tool's latest observation. In
orchestration, auto-fill only `matched` results — never invent values for
`ambiguous` or `no_match`. Pass `--refresh-snapshot` to force a re-fetch of
the snapshot from the MagicPay service.

On sensitive identity or payment pages, review matched profile autofills
before applying them. Use only target refs from the latest observation, and
refresh the snapshot when accuracy matters.

### `magicpay run-action <capability> [--item-ref <vaultItemId>] --params-json <json>`

Run a protected capability through the same MagicPay request model. This is
the canonical path for capabilities such as confirmation, provider-backed
authorization, or other protected actions that are not direct form fills.

Run only after the user approves the capability, params, site/merchant, and
visible amount or data for this task. Do not smuggle protected values through
free-form strings.

### `magicpay submit-form <fillRef>`

Manually submit the current protected form when `resolve-form` explicitly
leaves submission unfinished or when you intentionally retry on a fresh
protected-form snapshot.

Use only after explicit user approval for the current site/merchant, action,
and visible amount or data, and only after a fresh `find-form`/page check.
