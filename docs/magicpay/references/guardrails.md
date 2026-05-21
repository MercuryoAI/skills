# MagicPay Boundaries

## What This Skill Owns

- Start or continue the MagicPay product workflow session.
- Launch or attach an approved browser as a child resource inside that active
  workflow session.
- Discover the supported protected form.
- Resolve protected form targets through MagicPay request paths and fill them
  without submitting.
- Return post-fill browser continuation to the browser owner.
- Run typed protected action approvals through `authorize-payment`,
  `sign-message`, or `confirm-action`.
- Complete the MagicPay workflow with `magicpay end-session`, then return
  browser lifecycle decisions to the caller-owned browser tool or
  orchestrator. `magicpay close` closes or clears only the browser child.
- Recover from a confirmed real CAPTCHA on the current browser child with
  `solve-captcha`, then call `magicbrowse mark-captcha-resolved` before
  continuing through MagicBrowse when MagicBrowse owns the next step.

## Consequential Action Approval

Before any submit, protected action, purchase, login, identity submission,
account change, or other consequential action, get the matching typed
MagicPay approval for:

- the current site or merchant;
- the exact action to be taken;
- the visible amount, account, identity, or other data being submitted;
- whether the user wants final submission now.

MagicPay fills protected forms only. After `resolve-form <fillRef>`, continue
with the browser owner. If MagicBrowse produced a protected-form handoff, use
its `handoff.resumeObjective` for the next `magicbrowse act`. Do not treat a
filled protected form as approval to submit.

After typed approval, proceed with exactly that action; do not ask for a
second approval unless approved page facts changed. `authorize-payment` covers
the matching payment artifact use, payment form fill, and final Pay/Submit
while `amount`, `currency`, `recipient`, and `recurring` stay unchanged.
`sign-message` covers the exact message only. `confirm-action` covers only the
summarized non-payment consequential action.

## Readiness Rules

- Use `magicpay status` before a new protected-form task.
- If `status` reports a missing or invalid API key, run `magicpay init`.
- If `status` reports `cliUpdate`, use only
  `npm i -g @mercuryo-ai/magicpay-cli@latest`, then rerun `status`.
- Use `doctor` only when local config still looks broken after `init`.
- Normal product work starts with `magicpay start-session` before
  `magicpay launch` or `magicpay attach`.

Do not print, log, or share `MAGICPAY_API_KEY`, `~/.magicpay/config.json`, CDP
endpoints, or vault item ids. If the environment is shared or compromised,
stop and ask the user to revoke or rotate the key.

## Browser Authority

Use `magicpay launch` or `magicpay attach` only inside an active product
workflow session. Use `attach` only for the private browser/session the user
approved for this task. A CDP endpoint inherits the authority of any logged-in
browser state. Keep endpoints private and do not paste them into shared logs.
Run `attach` when MagicPay is not yet bound to the approved browser child, or
when the CDP endpoint changed. Re-attaching the same endpoint is allowed but
is not required as a ritual.

Browser teardown remains outside MagicPay's product-session authority.
`magicpay close` closes or clears the browser child while keeping the product
workflow active. If the browser was launched as an owned disposable session by
another tool, that tool can clean up after the overall task is done. If the
browser was external, user-owned, or handed to the user for inspection, leave
it open unless the user explicitly approves teardown.

## CAPTCHA Recovery

Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is
confirmed present on the current browser child inside the active product
workflow. Do not use it as page waiting, challenge detection, or a generic
retry.

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

- Start typed action commands only when an active product workflow session
  exists.
- Before `authorize-payment`, collect visible `amount`, `currency`,
  `recipient`, optional `description`, and optional `recurring` from the
  current page and the user's task.
- Prefer merchant/payee names over payment processor names. Use page title,
  host, or URL only as supporting signals unless they clearly identify the
  merchant.
- Ask the user when amount, currency, merchant/payee, recurring status, or
  task/page facts are missing, conflicting, or ambiguous.
- Use `magicpay authorize-payment` for payment authorization.
- Use `magicpay sign-message --item-ref <walletItemId> --message <text>` for
  wallet message signing, and ask again if the message changes.
- Use `magicpay confirm-action --summary <text> [--details <text>]` only for
  consequential actions without a more specific typed command.
- Keep `itemRef` on the existing selector path. Do not put it inside
  `params`, and do not change how MagicPay discovers or selects vault items.
- For approval handoff, add `--return-pending` to the typed action command,
  then either MagicPay UI approval plus `wait-request` or OTP confirmation
  plus `wait-request`.
- Do not ask for OTP until a pending approval request exists. OTP is optional,
  not a replacement for MagicPay UI approval.
- Do not print, log, summarize, save, or repeat OTP digits. Treat them as
  sensitive user input.

## Profile Match Rules

- `resolve-fields` refreshes the session-local snapshot of reusable
  open-data facts before matching.
- Use target ids from the latest observation only.
- Use `resolve-fields --request-missing` only for explicit observed fields
  that are required to move the user's task forward and whose meaning is clear.
- Do not request or fill optional newsletter, marketing, promo, survey,
  analytics, or similar fields.
- On sensitive identity or payment pages, review `matched` autofills before
  applying them.
- Leave `ambiguous` and `no_match` unresolved; do not invent replacements.
- After each protected or open-field fill, refresh the visible page state
  before deciding whether the task can continue.

## Secrecy And Safety

- Never type, print, summarize, or log protected values manually.
- Never type, print, summarize, or pass card PAN, CVV, wallet private keys,
  passwords, or other protected values through action params.
- Never print, log, summarize, or share `MAGICPAY_API_KEY`, local config, CDP
  endpoints, or vault item ids.
- Never include OTP digits in logs, reasoning summaries, saved notes, task
  reports, or command summaries.
- Base progress claims on the visible form state.
- After page-level changes, rerun `find-form` before acting on old form refs.

## Ask The User When

- a browser-dependent step is needed but there is no browser child and neither
  `magicpay launch` nor an approved private CDP endpoint is available;
- the browser/session to attach was not explicitly approved for this task;
- the next step would submit, login, purchase, send identity data, change an
  account, run a protected action, or otherwise commit a consequential action,
  and there is no matching typed approval for the unchanged current facts;
- payment authorization facts are missing or ambiguous: final amount,
  currency, merchant/payee recipient, recurring status, or a conflict between
  the user's task and the visible checkout page;
- the form remains ambiguous;
- approval reaches a terminal blocked state;
- a profile-field match on a sensitive identity or payment page needs review;
- client-side validation or merchant-specific recovery needs a human choice.
