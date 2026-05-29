# MagicPay Result States

MagicPay product work starts from an active workflow session created with
`magicpay start-session`. Browser-dependent states assume a browser child has
been launched or attached inside that product session.

## Reading CLI Results

For MagicPay JSON results, branch on fields, not on prose:

1. `success` — `true` means the command reached its typed success contract;
   `false` means a controlled blocked or failed outcome.
2. `outcomeType` — the command-specific success or blocked class.
3. `error` or `reason` — machine-readable subtype for blocked or terminal
   failure cases.

Use `message` and `reason` as text for the user or timeline. Do not parse them
to discover control flow.

## `plan-fill`

Read `plan-fill` results before calling `apply-fill`; apply only after the
active Memory fill plan is available.

Success shape:

```json
{
  "success": true,
  "plan": {
    "id": "plan_123",
    "valueVisibility": "handles_only",
    "fields": []
  },
  "nextAction": "apply-fill"
}
```

The plan is value-free. It may contain target refs, Memory field refs, safe
descriptor metadata, and value handles, but not raw saved values.

Blocked shape:

```json
{
  "success": false,
  "outcomeType": "blocked",
  "error": "matcher_unavailable",
  "message": "MagicPay could not complete the semantic Memory matcher request.",
  "reason": "The Memory matcher is unavailable through the current gateway configuration.",
  "nextAction": "ask-user"
}
```

`plan-fill.error` values:

- `browser_connection_failed` — the browser child is unreachable. `nextAction`
  is `attach`; rebind the approved browser/session before retrying.
- `page_resolution_failed` — the browser is reachable but the current page
  could not be resolved. Refresh or re-observe the browser state.
- `verification_required` — CAPTCHA, anti-bot, or human verification blocks
  planning. Use the CAPTCHA/auth handoff rules; do not retry through it.
- `redirect_loss` — checkout, booking, cart, or upstream redirect context was
  already lost. Stop; do not continue on a contextless page.
- `matcher_unavailable` — the semantic Memory matcher could not run. Fail
  closed and ask the user or retry only after gateway/tooling state changes.
- `match_ambiguous` or `match_unusable` — the model output could not be safely
  validated. Do not guess.
- `workflow_session_required` — there is no active MagicPay product workflow
  session. Start or restore the product session first.

## `apply-fill`

Read `plan-fill` results first, then call `apply-fill` only for the active
Memory fill plan.

Success shape:

```json
{
  "success": true,
  "status": "filled",
  "completedLedger": []
}
```

Branch on `success`, then `status`:

- `filled` — planned Memory values were filled. Refresh the browser state and
  continue with the browser owner. If the next browser action is consequential,
  get matching typed MagicPay approval.
- `page_changed` or `stale_plan` — the live page no longer matches the active
  plan. Rerun `magicpay plan-fill` on the current page before applying again.

Failure shape:

```json
{
  "success": false,
  "error": "active_plan_required",
  "message": "MagicPay could not find an active Memory fill plan.",
  "nextAction": "plan-fill"
}
```

Branch on `reason` and optional `error`:

- `denied`, `expired`, `failed`, `canceled`, `timeout` — stop the MagicPay
  path and report the exact state.
- `error: "active_plan_required"` — run `magicpay plan-fill` first.
- `error: "memory_materialization_failed"` — MagicPay could not materialize an
  approved value handle. Surface the blocker without exposing raw values.
- `error: "browser_fill_blocked"` — the browser fill layer refused the fill.
  Treat as blocked; refresh state before any retry.

## Request Paths

- `auto` — MagicPay resolved the request without waiting for a new user
  decision.
- `confirm` — MagicPay paused for explicit approval before using the protected
  data or action path.
- `provide` — MagicPay paused because the user needed to provide missing data
  or select the right item.
- terminal `denied`, `expired`, `failed`, `canceled`, or `timeout` — stop the
  MagicPay path and report the exact state.

### `session_stop`

A special variant of `canceled`: the whole workflow session was terminated
mid-flow by the user, a trust rule, or the backend. The result includes
`session_stop` details with a `code` and a human-readable `message`.

Do not retry the same request inside the same session. End the session with
`magicpay end-session`, then start a new one if the user wants to continue.

## Protected Actions

- `artifact` — a typed action command completed and returned the request
  artifact. Proceed with exactly that approved action; stop only if page facts
  changed.
- `pending` — a typed action command with `--return-pending` created the
  request and stored `currentRequestId`. The user can approve in MagicPay UI
  or provide the OTP they received. If they approve in UI, run `wait-request`;
  if they provide OTP, run `confirm-otp --otp <digits>` and then
  `wait-request`.
- `otp_invalid`, `otp_expired`, `otp_attempts_exceeded` — the OTP channel
  failed. Report the typed failure without repeating the OTP. While the
  request itself is still pending, keep MagicPay UI approval available.
- `request_already_resolved` — another channel already resolved the same
  pending request. Continue through `wait-request` or the returned terminal
  request state instead of applying a second decision.
