# Remote Workflow

```text
known checkout URL -> create_checkout_session -> host built-in browser
known raw x402 resource -> run_x402_payment -> MagicPay payment run
unknown target -> search_provider_methods -> agent verifies docs and executes
material shortlist -> existing or general session -> one normalized choice request
choice answer in any channel -> same exact request result -> selected branch only
paid provider method -> exact current request -> MagicPay payment run
checkout -> host browser ordinary work -> remote approval
approval -> same-run one-time card and ordinary values -> native browser fill
fill -> one exact native browser click -> immediate value-free result record
dispatch -> one fresh merchant result check
fully projected composed result -> terminal workflow without a second closer
terminal workflow error -> fail_checkout_session -> terminal workflow
payment operation -> independent pending or settled reconciliation truth
```

Native operations do not create browser checkout approval. An exact x402
resource uses `run_x402_payment`, whether the agent already knew it or found the
provider method through MagicSearch. Registry prose is not payment authority:
the agent verifies current documentation and supplies the exact current request
under current user authority or MagicPay policy. New crypto work uses
`run_crypto_transfer`. All three use the same composed `wait_payment`
continuation:

```text
exact x402 facts + stable clientRequestId -> composed payment run
exact crypto facts + stable clientRequestId -> composed payment run
exact existing browser session + stable clientRequestId -> composed browser payment run
native run waiting_for_user -> exact operation-owned request -> same-run bounded wait -> one approved/executing handoff -> same-run final wait
browser run ready_for_browser -> native fill -> one final action -> fresh validation/submission observation -> record_browser_payment_result
run completed -> durable transfer evidence or verified seller result
operation awaiting approval -> exact operation-owned request continuation
approved operation -> same-operation execution or reconciliation
external_pending direct transfer -> successful submission -> background confirmation -> callback notification
completed operation -> settlement truth (plus bounded result for x402)
```

For a composed run, report the returned `request_url` and immediately call
`wait_payment` on the same `runId` and progress cursor. It polls every three
seconds for up to 270 seconds. A direct-transfer or x402 waiter may return once
when it observes the new approved/executing handoff. Acknowledge receipt of the
approval, then immediately call `wait_payment` again with the same `runId` and
returned cursor so the already-approved operation continues in the same run.
The cursor makes that acknowledgement one-time; terminal, action-required, and
`external_pending` results take precedence. A composed browser run does not use
this native-operation handoff: keep its long-lived waiter on the same browser
run until it reaches its browser action, terminal, or action-required boundary.
For generic request inspection or recovery, follow [requests.md](requests.md).
The user decides payment approval in the MagicPay approval system; a plain
chat confirmation cannot substitute for it. Browser approval belongs only to
the composed direct browser run.

Within one unchanged connected task, a successful authenticated setup,
capability, or payment call establishes readiness. The unscoped balance call
or composed payment run is sufficient for its native preparation; never
insert another `get_magicpay_capabilities` call before exact waiting, result,
or reconciliation continuations. Invalidate only after connection, plugin,
environment, credential, or authentication/configuration changes or failures;
do not persist readiness or apply a TTL.

Every recovery call retains the same exact draft, run, session, request,
approval, receipt, attempt, and operation identity unless the backend returns a
terminal state and the user explicitly begins a new intent. Approval expiry,
cancellation, transport ambiguity, provider submission, and a held reservation
do not permit replacement.

Before `record_browser_payment_result`, let the host browser naturally complete
all ordinary and intermediate interactions. Payment-method selection, controls
that reveal or advance the form, and corrections after dynamic page changes are
not payment commitment and do not consume the one-shot dispatch boundary. If
the host cannot confidently reach the actual dispatch control, hand the exact
tab to the user with a page-specific reason before any possible dispatch.

After `record_browser_payment_result`, check the fresh merchant result once. An
agent-direct result records activation of the actual payment-dispatch control
and submission separately: a fresh visible merchant validation blocker is
`clicked` plus `not_submitted`, while an unreadable post-dispatch state is
`submission_unknown` and never replayable. The merchant-confirmed outcome does
not make settlement successful. Keep the exact
operation `pending` until provider or operation evidence changes it. When the
composed browser result is fully projected and returns `completed`, its session
is already terminal; do not call `complete_checkout_session` as a second closer.
Use that tool only when an exact current continuation directs closure. For
cancellation, consume the backend's `cleanupDisposition` and
`freshStartAllowed`; do not infer cleanup or permission for a fresh attempt.

For a terminal workflow failure, call `fail_checkout_session` once with the
exact session, stable idempotency key, and observed failure reason. Consume its
cleanup and reconciliation output exactly. A failed workflow may still retain
one possibly dispatched operation for same-operation reconciliation; it never
permits deletion, replay, or replacement by itself.
