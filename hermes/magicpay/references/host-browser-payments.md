# Direct Browser Payments

Use this path when `browserPaymentRun.status: "ready"` and
`executionMode: "agent_direct"`. The host's built-in Browser owns the live tab.
MagicPay owns the exact approval, operation, payment-scoped card, result, and
reconciliation state. Do not add a second browser controller.

## One run, one approval

Create the checkout session for the known destination. Use the Browser to reach
the actual payment-dispatch surface without activating it, then observe the
merchant/recipient, amount and maximum debit, currency, recurrence, payment
type, actual final-action meaning, and the ordinary semantic roles visible now.

Call `run_browser_payment` once with one stable `clientRequestId`. Do not wrap it
in separate balance, capability, approval, or card-preparation calls. Approval
creates one short checkout authority for the unchanged merchant and payment.
If email, name, phone, country, billing address, city, region, or postal code
appears later, add it to the sorted unique role union and replay the same run.
Never remove a previously observed role. Passwords, OTPs, identity documents,
tax identifiers, bank credentials, private keys, seeds, and unrelated secrets
are outside this authority and require a safe user handoff.

| Run state | Continue with |
| --- | --- |
| `waiting_for_user` | Present the exact approval URL; wait on the same `runId`. |
| `ordinary_field_required` | Resolve only the returned roles and resume the same run; this is not a new payment approval. |
| `preparing_browser` | Wait on the same run. |
| `ready_for_browser` | Fill the unchanged checkout in the exact live tab. |
| `external_pending` | Reconcile the same operation; never click again. |
| `reconciliation_required` | Reconcile the exact operation; never replace it. |
| `completed` | Report success only from the durable completed operation. |

## Browser ownership and rendered state

Give the built-in Browser the unchanged approved checkout and let it use the
page's normal flow, normal visual understanding, and interaction capabilities.
Do not prescribe selectors, button names, page geometry, a fixed field order,
or a scripted interaction sequence. The Browser may inspect, scroll the page or
a nested checkout container, focus, select, click, type, correct, revisit, and
re-check ordinary or intermediate controls as the rendered page requires.

Payment-method selectors, custom radio controls, accordions, next/continue
controls, and buttons that reveal or advance a form are intermediate. A control
labelled Donate, Support, Checkout, or Pay can still be a form opener. Activate
and correct those controls naturally before protected fill.
After each material transition, discard stale targets, re-observe the rendered
page, and reacquire current targets. If the transition reveals an empty card
form or a later dispatch control, the action that caused it was not payment
commitment and must not be recorded as `clicked` or `click_uncertain`.

Rendered visibility is required before protected fill.
Hidden DOM or accessibility presence is not rendered visibility.
A semantic label, Boolean value presence, an enabled hidden control, a click,
or a content mutation does not prove that the actual payment-dispatch surface
is rendered or that payment was submitted. When a custom payment-method
control, collapsed payment panel, nested checkout, or material transition
leaves the structured state ambiguous, take native screenshots as often as
needed.
Start taking screenshots before any returned card character is entered, then
keep using them whenever visual understanding helps. Use them to confirm the
selected method, rendered card form, currently rendered dispatch control, and
visible amount, recurrence, and optional extras.
If a screenshot and structured state disagree, the rendered page controls
visibility; re-observe and reacquire instead of dispatching. A screenshot is
perception input only; it never approves, submits, retries, or proves settlement.

Keep navigation, ordinary fill, protected fill, validation, and dispatch as
separate native Browser actions. Fill the merchant's remaining ordinary fields;
its checkout email is authoritative and an optional Stripe Link email is not a
second required role. Fill card fields last with typed native sensitive-fill
actions. Then verify value presence, accessible merchant validation, and the
already identified rendered final control before invoking that exact control in
one isolated Browser call. Boolean or accessibility state alone is insufficient
to identify a hidden final control.

After a rerender, refill only an allowed field with value-free evidence that it
is empty. One targeted refill is enough; do not loop, sweep every `:invalid`
element, or treat unreadable iframe formatting and optional fields as blockers.

Supply the returned card only through exact typed sensitive-fill actions for
this approved tab.
Native screenshots are allowed before and after protected fill.
An incidental view of the payment fields or one-time card in that exact tab is
allowed. Use screenshots freely whenever they improve rendered-page
understanding; do not reduce visual inspection merely because protected fill
already happened. Keep every screenshot transient inside Browser reasoning.
Never export, attach, quote, OCR, log, persist, or reuse it, and never place card
values in chat, general JavaScript source, shell or CLI arguments, files, events,
logs, analytics, or evidence. Do not combine card fill with navigation or
dispatch.

## Pre-dispatch Browser interruption

A tab binding can become stale without invalidating the Browser binding. When a
fill/navigation call is definitely before the isolated final action, do not
call `record_browser_payment_result`, fail, cancel, release, or create another
run. Perform at most one recovery cycle:

1. discard only the stale tab binding;
2. reacquire the exact live tab from the existing Browser binding, or reopen
   the same approved HTTPS checkout when that tab no longer exists;
3. replay the same `clientRequestId` and `runId`;
4. re-observe and refill only missing allowed fields; and
5. continue with the same execution-attempt ID.

Only an explicit Browser-disconnected error invalidates the Browser binding.
If one recovery cycle cannot restore the unchanged pre-dispatch checkout, hand
the exact checkout to the user without replacing the payment.

If the interruption overlaps the isolated final-action call, record
`click_uncertain` and reconcile the same operation. Never reopen for another
click. A timeout or missing output is not permission to retry dispatch.

## Challenges and unusual states

Load detailed recovery guidance only when the matching state occurs. The
Browser may try to solve a CAPTCHA once before dispatch, then hand the same tab
to the user if needed. A CAPTCHA before dispatch does not fail or replace the
payment. Invoice handling is optional and
never blocks settlement. A merchant validation blocker permits correction in
the same run. After any possible dispatch, observation is read-only and the
same operation must be reconciled.

## Record the result once

Call `record_browser_payment_result` once for the exact run and execution
attempt:

- `not_clicked` only when the isolated payment-dispatch control definitely was
  not activated;
- `clicked` when it was activated, with the separately observed submission and
  merchant outcome; or
- `click_uncertain` when activation may have occurred.

Pass an ordinary receipt address only in `checkoutEmail`, never in
`valueFreeEvidence`. Approval, card materialization, field fill, click, and
merchant visibility are not settlement. When the composed result is
`completed`, stop; do not call a second checkout closer.
