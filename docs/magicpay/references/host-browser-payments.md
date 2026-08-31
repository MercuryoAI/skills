# Direct Browser Payments

This is the only browser-payment path. Use it when
`browserPaymentRun.status: "ready"` and `executionMode: "agent_direct"`. The
host's built-in browser owns the live page, including multi-step navigation,
ordinary and payment fields, challenges, the one final commitment, and fresh
result interpretation.

## Composed run

Create the checkout session for the exact destination and keep its `sessionId`.
Before the first `run_browser_payment` call, use the built-in browser's normal
visual understanding and ordinary interactions to reach the actual
payment-dispatch surface without activating it. Complete ordinary and
intermediate navigation that reveals later form sections, using ordinary values
already available in the current task. If a missing ordinary value is needed to
reveal the next pre-dispatch step, ask only for that ordinary value in regular
chat. Re-observe after each material transition until the
actual payment-dispatch surface and the complete page-derived semantic
ordinary-field roles are known. This is natural form navigation, not a blanket
required-field sweep; do not infer merchant-specific roles or prescribe
selectors.

Only then call `run_browser_payment` with that session, one stable
`clientRequestId`, the approved amount and maximum debit, the observed payment
facts, and the complete ordinary-field role set in sorted, unique, stable order.
Do not create a run from an initial partial form and add roles later. Do not add
separate balance, capability, approval, or card-preparation calls around it.

Follow the returned state:

- `waiting_for_user`: present the exact approval URL and wait on the same
  `runId` with `wait_payment`.
- `ordinary_field_required`: use saved Memory first. If it is unavailable or
  does not work, ask only for the returned ordinary roles in regular chat,
  submit them to that exact request, and continue the same run. This is not a
  payment failure and creates no replacement checkout.
- `preparing_browser`: keep waiting on the same run.
- `ready_for_browser`: give the unchanged approved checkout to the built-in
  browser with the returned ordinary values and payment-scoped one-time card.
- `external_pending`: the submission is recorded; reconcile the same operation
  and never retry the checkout.
- `completed`: report success only from the returned durable completed
  operation evidence.
- `reconciliation_required`: reconcile the exact operation; never replace it.

Approval is the user's authorization for the unchanged payment facts, approved
ordinary-data transmission, returned one-time card, and one final commitment.
Do not ask for another payment or data-sharing confirmation.

## Browser handoff

Give the built-in browser the unchanged approved outcome and let it use the
page's normal flow, its normal visual understanding, and its normal interaction
capabilities. Do not prescribe
selectors, button names, page geometry, a fixed field order, or a scripted
interaction sequence. Forms may be multi-step, embedded, redirected, dynamic,
or use custom controls; the browser may inspect, scroll, focus, select, click,
type, correct, revisit, and re-check ordinary or intermediate controls as often
as the live page requires.

Intermediate interactions are not payment commitments. Payment-method tabs,
radio-like custom controls, accordions, next/continue controls, address
selectors, validation controls, and buttons that merely reveal another form may
be activated and corrected naturally. The one-shot boundary begins only at the
actual merchant control that may dispatch the approved payment. A semantic
label, DOM click, content mutation, busy state, or newly revealed payment form
alone does not prove that boundary was reached or that payment was submitted.

Before transmitting payment-card values, resolve page choices that affect the
approved facts. The browser remains free to revisit those choices if the page
changes later. Before the actual payment-dispatch control, use fresh visual understanding to
preserve the approved amount, recurrence, and optional extras and to confirm
that dynamic page changes did not clear required ordinary fields or deactivate
the chosen payment method. This is an outcome check, not a fixed checklist or
interaction recipe.

If the browser cannot confidently understand or reach the actual dispatch
control, preserve the exact tab and hand it to the user before attempting that
control. Explain the page-specific reason, say that MagicPay is working to
improve these interactions, and state that security remains the top priority.
Do not describe the handoff as a payment failure or ask for another MagicPay
approval. When submission is definitely still pre-dispatch, the user may review
the unchanged checkout and perform the remaining final action. After a possible
dispatch, the handoff is observation-only and must not invite another click.

Screenshots and visual inspection are allowed. Screenshot hardening can be
added later after the direct flow is stable.

The returned payment-card values stay in structured tool context and may be
used only in this exact approved checkout. Never quote them in chat, logs,
source, or command arguments.

## CAPTCHA

A CAPTCHA is action required, not an error, payment failure, or settlement
evidence. Let the built-in browser solve it first under its normal capabilities.
If it cannot, hand the same tab to the user in the host's normal way. Preserve
the same run and unchanged approved facts, then continue only after a fresh
observation shows the challenge is cleared. Do not ask for another MagicPay
confirmation. Do not fail, cancel, release, replace, or record non-submission
merely because a CAPTCHA appeared.

If a challenge appears after possible payment dispatch or the dispatch is uncertain,
do not click again. Record the observed uncertainty and reconcile the same
operation.

## Result

Call `record_browser_payment_result` once for the exact run and operation:

- `not_clicked` only when the payment-dispatch control definitely was not
  activated;
- `clicked` when the payment-dispatch control was activated, with separately
  observed submission and merchant outcome;
- `click_uncertain` only when payment dispatch may have been activated. This is
  never replayable.

Do not call an intermediate interaction `clicked` or `click_uncertain`. Continue
the normal browser flow instead. A visible merchant validation message can prove
`not_submitted`; silence, loading, hidden state, or an unreadable page cannot.
Once the actual payment-dispatch control may have been activated, `clicked` and
`click_uncertain` are never replayable.

When the composed result returns `completed`, the same session is already fully
projected and terminal. Stop there; do not call `complete_checkout_session` as
a second closer. That tool remains available for legacy and other non-composed
workflow closure. Approval, card creation, form fill, click, and merchant
visibility are not settlement by themselves.

Reuse the same `clientRequestId`, `runId`, execution attempt, and result identity
for unchanged continuation. A timeout, lost response, approval pause, CAPTCHA,
or missing observation never authorizes a replacement run or second click.
