# MagicPay Command Guide

The hard rules from `SKILL.md` apply to every command: protect the
MagicPay API key and CDP endpoint, use only the browser/session approved
for this task, keep protected form resolution to fill-only, and
ask for explicit approval before any submit, protected action, purchase,
login, identity submission, account change, or other consequential action.

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

## Browser Attach And Session Control

### `magicpay attach <cdp-url> [--provider <name>]`

Connect MagicPay to an already running browser through CDP.

Use only a private CDP endpoint for the prepared browser/session the user
approved for this task. Treat the endpoint as sensitive because it inherits
the browser's logged-in state. Run `attach` when MagicPay is not yet bound to
the approved prepared browser/CDP session, or when the CDP endpoint changed.
Re-attaching the same endpoint is allowed but is not required as a ritual.

### `magicpay solve-captcha [--timeout <s>]`

Solve a confirmed CAPTCHA on the current attached browser page.

Only call this when a real CAPTCHA is confirmed present. The command uses the
current MagicPay-attached browser session, does not require `start-session`,
and does not close or recreate the browser. After a successful solve, continue
the ordinary browser or protected-form flow from the current page. If the next
step is through MagicBrowse, call `magicbrowse mark-captcha-resolved`, then
continue with `magicbrowse act "continue..."`.

### `magicpay start-session [name] [--merchant-name <name>]`

Bind the attached browser to a MagicPay workflow session.
`start-session` attempts to cancel/clear a stale previous workflow binding
before it creates the new session. If that recovery is still blocked, start
manual recovery with `magicpay status`, then either `magicpay end-session` or
a fresh `attach` / `start-session` on the approved browser.

### `magicpay end-session`

Complete the active workflow session without closing the browser.

This is workflow completion only. After it succeeds, return control to the
caller-owned browser lifecycle. A browser tool or orchestrator that launched
an owned disposable browser may clean up its own session when the overall task
is done; an external/user-owned browser stays open unless the user explicitly
approves teardown.

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

On sensitive identity or payment pages, review matched profile autofills
before applying them. Use only target ids from the latest observation.

### `magicpay run-action <capability> [--item-ref <vaultItemId>] --params-json <json>`

Run a protected capability through the same MagicPay request model. This is
the canonical path for capabilities such as confirmation, provider-backed
authorization, or other protected actions that are not direct form fills.

Run only after the user approves the capability, params, site/merchant, and
visible amount or data for this task. The capability must come from the
current form, vault, or action context; do not invent a free-form capability
name or smuggle protected values through free-form strings.
