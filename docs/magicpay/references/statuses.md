# Status Meanings

- `clarification_required`: ask only the returned safe fields.
- `ready`: start the exact remote discovery revision.
- `choice_required`: select only an offered opaque choice ID.
- `resolved`: atomically create the checkout session.
- `in_progress` / `waiting_for_user`: keep the same session.
- `envelope_issued`: apply once in the same prepared host session.
- `filled`: finalize the value-free proof.
- `staged`: the consequential click has not yet been durably classified.
- `clicked`: dispatch occurred; settlement remains unknown.
- `click_uncertain`: dispatch may have occurred; reconcile and never replay.
- "merchant_confirmed": the merchant visibly confirmed the submitted checkout.
  Close the workflow with structured `checkoutOutcome`; provider settlement may
  remain pending on the exact operation.
- `awaiting_approval`: retain stable operation-owned `approval.requestId`, then
  continue the same remote session using the distinct routable UUID
  `approval.runtimeRequestId`; do not create another approval.
- Payment-run `running`: carry the same `runId` and `nextProgressCursor` into
  `wait_payment`; it is neither failure nor permission to restart.
- Payment-run `waiting_for_user`: report the exact returned request URL and,
  immediately call `wait_payment` on the same run; its one bounded call polls
  every three seconds while approval is open. Never require a chat reply or
  create a replacement request.
- Payment-run `reconciliation_required`: reconcile only its exact returned
  operation. A missing x402 result after financial completion does not permit
  another purchase.
- funding_required: only authoritative insufficient unified user balance
  automatically opens `show_topup`. Durable top-up settlement may wake only the
  same approved, unchanged, definitely-not-submitted operation; continue its
  same run and never replace it.
- service_unavailable with card-pool insufficiency: this is a temporary card
  service capacity issue, not a user-balance funding request. Do not call any
  top-up or fresh payment tool; direct the user to support@magiccard.ai.
- `revoked`: canceled pre-submit approval authority. The historical approval
  decision may remain auditable, but it cannot resume, materialize, or authorize
  any payment operation.
- `external_not_submitted`: follow only the exact operation's returned retry
  guidance; provider non-submission is not permission to create a replacement.
- `external_pending`: a direct transfer was submitted successfully and is in
  normal blockchain confirmation. Report that no action is needed and MagicPay
  will notify the user after settlement, stop foreground polling, and retain the
  exact operation/session. Only `completed` proves settlement.
- `reconciliation_required`: reconcile the same operation only.
- `completed`: terminal only when the operation/provider evidence agrees.
- `definitively_failed`: terminal for that operation attempt. Do not retry when
  `retry.allowed:false` or the failure is non-retryable. After the owning
  workflow is durably closed, a later explicit user request may create a wholly
  new payment only when the closure also returns `released_pre_submit`, failed
  or not-started settlement, `freshStartAllowed: true`, and `nextAction: none`.
- `canceled`: terminal for workflow authority immediately. A separately
  preserved dispatched or uncertain operation may still be nonterminal and
  require same-operation reconciliation; cancellation does not release its
  held Ledger reservation by itself. Trust only returned `cleanupDisposition`
  and `freshStartAllowed` for cleanup and fresh-attempt policy.
- `cleanup_pending`: the workflow is canceled but the exact operation release is
  not verified. Replay only the same cancellation as directed by
  `retry_cleanup_same_operation`.
- `failed`: the workflow closes through `fail_checkout_session` with one exact
  failure code and stable idempotency key. A separately unresolved or possibly
  dispatched operation remains bound to the returned same-operation
  reconciliation action; failure alone does not release it or authorize a new
  payment. Only the complete safe fresh-start disposition above plus a later
  explicit user request creates new authority, with all-new identities.

Status, cancellation, and reconciliation reads remain silent supporting calls.
An error, hard stop, separately pending reconciliation, or required user action does not
request a widget; report it in normal conversation unless the user separately
asks for the corresponding view. The sole exception is normalized
funding_required from authoritative unified user-balance insufficiency.
