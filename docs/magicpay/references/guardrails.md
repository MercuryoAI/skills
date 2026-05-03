# MagicPay Boundaries

## What This Skill Owns

- Attach to a prepared browser page.
- Start or continue the workflow session for that page.
- Discover the supported protected form.
- Resolve protected form targets through MagicPay request paths and fill them
  for review.
- Submit protected forms only after explicit approval for the current
  site/merchant, action, and visible amount or data.
- Run protected capabilities through `run-action` only after explicit approval
  for the capability and params.
- Retry submit only when the guarded fill path explicitly leaves work undone
  and the user approves that retry.

## Consequential Action Approval

Before any submit, protected action, purchase, login, identity submission,
account change, or other consequential action, confirm:

- the current site or merchant;
- the exact action to be taken;
- the visible amount, account, identity, or other data being submitted;
- whether the user wants final submission now.

Use non-submit review flows first whenever available. In orchestrated skill
workflows, that means `resolve-form <fillRef> --no-submit` first, then
`submit-form <fillRef>` only after approval and a fresh page/form check. Do
not treat guarded submit support as permission to submit without user
approval.

## Readiness Rules

- Use `magicpay status` before a new protected-form task.
- If `status` reports a missing or invalid API key, run `magicpay init`.
- If `status` reports `cliUpdate`, use only
  `npm i -g @mercuryo-ai/magicpay-cli@0.1.10`, then rerun `status`.
- Use `doctor` only when local config still looks broken after `init`.

Do not print, log, or share `MAGICPAY_API_KEY`, `~/.magicpay/config.json`, CDP
endpoints, or vault item ids. If the environment is shared or compromised,
stop and ask the user to revoke or rotate the key.

## Browser Authority

Use `magicpay attach` only for the prepared browser/session the user approved
for this task. A CDP endpoint inherits the authority of any logged-in browser
state. Keep endpoints private and do not paste them into shared logs.

## Protected-Form Rules

- Start from a current `find-form` result, not from stale assumptions.
- Do not call `resolve-form` on a stale `fillRef`.
- Use `--item-ref` only when you intentionally want one specific vault item.
- Use `resolve-form <fillRef> --no-submit` by default in orchestrated skill
  workflows.
- Treat `submit-form` as the explicit approved submit step or manual recovery,
  not as the default next step.
- If `resolve-form` reports stale bindings or no live submit control, refresh
  the page state before retrying.

## Protected-Action Rules

- Start `run-action` only when an active workflow session exists.
- Start `run-action` only after the user approves the capability and params
  for the current site/merchant and visible amount or data.
- Provide structured JSON params to `run-action`; do not smuggle protected
  values through ad-hoc strings or prompts.
- Use `run-action` for protected capabilities instead of inventing a manual
  form-fill equivalent.

## Profile Match Rules

- `resolve-fields` uses a session-local snapshot of reusable open-data facts.
- Use target refs from the latest observation only.
- On sensitive identity or payment pages, refresh the snapshot and review
  `matched` autofills before applying them.
- Leave `ambiguous` and `no_match` unresolved; do not invent replacements.

## Secrecy And Safety

- Never type, print, summarize, or log protected values manually.
- Never print, log, summarize, or share `MAGICPAY_API_KEY`, local config, CDP
  endpoints, or vault item ids.
- Base progress claims on the visible form state.
- After page-level changes, rerun `find-form` before acting on old bindings.

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
