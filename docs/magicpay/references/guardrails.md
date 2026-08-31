# Guardrails

- The host browser is the only page-control owner.
- Direct-browser values appear only after finalized approval and must be used
  only in the exact native-browser checkout.
- Durable state is remote. Keep no second local workflow record.
- A changed URL, document, frame, evidence, amount, currency, recipient,
  recurrence, country, or final control invalidates prior page authority.
- Never expose payment-scoped one-time card details or opaque envelopes in logs,
  chat, screenshots, source, or command arguments. Their structured tool output
  may be used only for native fill of the exact approved checkout. The final
  seller deliverable from a completed purchase is user-owned
  output, not protected input, even when a seller names it `credentials`,
  `token`, `key`, or `code`.
- Keep an explicitly seller-declared continuation capability out of the final
  response; use it only to retrieve the final seller deliverable.
- Never treat approval, protected fill, click, page copy, or HTTP transport as
  settlement.
- Never replace an ambiguous operation or replay a possibly dispatched click.
- Let the host browser use its normal visual and interaction capabilities for
  ordinary and intermediate page controls. Do not turn payment-method selection,
  form revelation, navigation, validation, or correction into a payment result.
- After the actual payment-dispatch control may have been activated, call
  `record_browser_payment_result` immediately. From that boundary, `clicked` and
  `click_uncertain` are never replayable.
- Bind native approval to the exact intent session, operation ID, stable
  operation-owned `approval.requestId`, and routable UUID
  `approval.runtimeRequestId`. Continue only through the composed run and its
  generic exact request; never route with the mpr_ identity or create a
  substitute approval.
- Preserve atomic quantities, direct-transfer facts, x402 URL/method/body, and
  the original idempotency key exactly. Same-operation recovery is the only
  retry path.
- Only authoritative unified user-balance funding_required may automatically
  show top-up. Card-pool insufficiency is a temporary service issue and never
  authorizes `show_topup`, a hosted top-up link, or a replacement payment.
- Funding recovery may resume only the exact operation after durable same-user
  settlement, unchanged facts and fingerprint, valid approval, and definite
  non-submission. A failed wake remains retryable coordination and cannot change
  top-up settlement truth.
- A held reservation remains held until the exact backend operation and Ledger
  projection release it. Approval expiry and cancellation are not release
  evidence.
- Honor cancellation before recovery work. A successful cancellation closes
  workflow authority even when a possibly dispatched operation remains held;
  reconcile that exact operation separately and never replay it.
- `retry.allowed:false`, non-retryable failure, and terminal remediation are
  hard stops for that exact operation. Do not retry it, replace it, submit it to
  a provider, or click a merchant control to bypass the stop. A separately
  user-authorized new intent is permitted only after terminal failure closure
  explicitly returns `released_pre_submit`, failed or not-started settlement,
  `freshStartAllowed: true`, and `nextAction: none`; never reuse any old
  identity or authority.
