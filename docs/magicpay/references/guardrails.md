# MagicPay Boundaries

## What This Skill Owns

- Attach to a prepared browser page.
- Start or continue the workflow session for that page.
- Discover the supported protected form.
- Resolve protected form targets through MagicPay request paths and fill them
  without submitting.
- Return post-fill browser continuation to the browser owner.
- Run protected capabilities through `run-action` only after explicit approval
  for the capability and params.
- Complete the MagicPay workflow with `magicpay end-session`, then return
  browser lifecycle decisions to the caller-owned browser tool or
  orchestrator. MagicPay does not close the browser.
- Recover from a confirmed real CAPTCHA on the current attached browser page
  with `solve-captcha`, then call `magicbrowse mark-captcha-resolved` before
  continuing through MagicBrowse.

## Consequential Action Approval

Before any submit, protected action, purchase, login, identity submission,
account change, or other consequential action, confirm:

- the current site or merchant;
- the exact action to be taken;
- the visible amount, account, identity, or other data being submitted;
- whether the user wants final submission now.

MagicPay fills protected forms only. After `resolve-form <fillRef>`, continue
with the browser owner. If MagicBrowse produced a protected-form handoff, use
its `handoff.resumeObjective` for the next `magicbrowse act`. Do not treat a
filled protected form as approval to submit.

## Readiness Rules

- Use `magicpay status` before a new protected-form task.
- If `status` reports a missing or invalid API key, run `magicpay init`.
- If `status` reports `cliUpdate`, use only
  `npm i -g @mercuryo-ai/magicpay-cli@latest`, then rerun `status`.
- Use `doctor` only when local config still looks broken after `init`.

Do not print, log, or share `MAGICPAY_API_KEY`, `~/.magicpay/config.json`, CDP
endpoints, or vault item ids. If the environment is shared or compromised,
stop and ask the user to revoke or rotate the key.

## Browser Authority

Use `magicpay attach` only for the prepared browser/session the user approved
for this task. A CDP endpoint inherits the authority of any logged-in browser
state. Keep endpoints private and do not paste them into shared logs.
Run `attach` when MagicPay is not yet bound to the approved prepared
browser/CDP session, or when the CDP endpoint changed. Re-attaching the same
endpoint is allowed but is not required as a ritual.

Browser teardown remains outside MagicPay's authority. If the browser was
launched as an owned disposable session by another tool, that tool can clean
up after the overall task is done. If the browser was external, user-owned,
or handed to the user for inspection, leave it open unless the user explicitly
approves teardown.

## CAPTCHA Recovery

Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is
confirmed present on the current attached page. Do not use it as page waiting,
challenge detection, or a generic retry.

When the next step is owned by MagicBrowse and the solve succeeded, call
`magicbrowse mark-captcha-resolved`, then continue with
`magicbrowse act "continue..."`. The marker only tells MagicBrowse that an
external participant resolved CAPTCHA for this page; MagicBrowse still checks
the actual page state and must stop again if CAPTCHA or human verification is
still visible.

## Protected-Form Rules

- Start from a current `find-form` result, not from old assumptions.
- Do not call `resolve-form` on an old `fillRef` after page changes.
- Use `--item-ref` only when you intentionally want one specific vault item.
- Use `resolve-form <fillRef>` to fill and stop.
- If `resolve-form` reports `form_changed`, refresh the page state and rerun
  `find-form` before retrying.

## Protected-Action Rules

- Start `run-action` only when an active workflow session exists.
- Start `run-action` only after the user approves the capability and params
  for the current site/merchant and visible amount or data.
- Use only a capability discovered from the current form, vault, or action
  context. Do not invent a free-form capability name.
- Provide structured JSON params to `run-action`; do not smuggle protected
  values through ad-hoc strings or prompts.
- Use `run-action` for protected capabilities instead of inventing a manual
  form-fill equivalent.

## Profile Match Rules

- `resolve-fields` refreshes the session-local snapshot of reusable
  open-data facts before matching.
- Use target ids from the latest observation only.
- On sensitive identity or payment pages, review `matched` autofills before
  applying them.
- Leave `ambiguous` and `no_match` unresolved; do not invent replacements.

## Secrecy And Safety

- Never type, print, summarize, or log protected values manually.
- Never print, log, summarize, or share `MAGICPAY_API_KEY`, local config, CDP
  endpoints, or vault item ids.
- Base progress claims on the visible form state.
- After page-level changes, rerun `find-form` before acting on old form refs.

## Ask The User When

- the prepared page context is missing;
- the prepared browser/session was not explicitly approved for this task;
- the next step would submit, login, purchase, send identity data, change an
  account, run a protected capability, or otherwise commit a consequential
  action;
- the form remains ambiguous;
- approval reaches a terminal blocked state;
- a profile-field match on a sensitive identity or payment page needs review;
- client-side validation or merchant-specific recovery needs a human choice.
