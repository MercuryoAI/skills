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

## `find-form`

Success shape:

```json
{
  "success": true,
  "outcomeType": "form_found",
  "pageRef": "p0",
  "url": "https://merchant.example/checkout",
  "title": "Checkout",
  "host": "merchant.example",
  "purpose": "payment_card",
  "formCount": 1,
  "selectedFillRef": "f1",
  "selectionReason": "single_supported_form_on_page",
  "nextAction": "resolve-form",
  "forms": []
}
```

`forms[].fields[].valueHint` is a semantic binding hint such as `direct` or
`full_name.given`, not a protected value preview. `forms[].candidateItems[]`
contains operational `itemRef` selectors and sanitized `displayName` metadata.
Do not show `itemRef` to the user.

Blocked shape:

```json
{
  "success": false,
  "outcomeType": "blocked",
  "error": "verification_required",
  "message": "Verification is required before the protected form can be used.",
  "reason": "MagicPay detected a verification or anti-bot challenge on the current page.",
  "nextAction": "ask-user"
}
```

`find-form.error` values:

- `browser_connection_failed` — the browser child is unreachable. `nextAction`
  is `attach`; rebind the approved browser/session before retrying.
- `page_resolution_failed` — the browser is reachable but the current page
  could not be resolved. Refresh or re-observe the browser state.
- `verification_required` — CAPTCHA, anti-bot, or human verification blocks
  discovery. Use the CAPTCHA/auth handoff rules; do not retry through it.
- `redirect_loss` — checkout, booking, cart, or upstream redirect context was
  already lost. Stop; do not continue on a contextless page.
- `matcher_unavailable` — the semantic matcher could not run. Fail closed and
  ask the user or retry only after the page/tooling state changes.
- `protected_form_not_found` — no supported login, identity, or payment-card
  form is present. Verify the page context before retrying.
- `protected_form_match_unusable` — form-like fields were found, but the match
  plan is unsafe. Read `diagnostics`; do not fill protected data.
- `protected_form_ambiguous` — multiple supported forms match. Surface the
  ambiguity and ask the user to choose.

`diagnostics` for `protected_form_match_unusable`:

- `problem: "unsafe_match_plan"`
- `explanation` — safe human-readable summary
- `acceptedFields[]` and `rejectedFields[]` — field keys, target refs, labels,
  value hints, and rejection reasons
- `safeNextActions[]` — machine-readable safe recovery hints, currently
  `observe_again`, `use_resolve_fields_for_required_open_fields`, and
  `stop_if_still_unsafe`

## `resolve-fields`

Success shape:

```json
{
  "success": true,
  "outcomeType": "fields_resolved",
  "pageRef": "p0",
  "url": "https://merchant.example/checkout",
  "host": "merchant.example",
  "refreshedSnapshot": true,
  "results": []
}
```

Each `results[]` item has one of these statuses:

- `matched` — includes `targetRef`, `fieldKey`, `value`, `source`, and
  `confidence`. Use `value` only to fill the current field; do not repeat it
  in chat, logs, task reports, or summaries.
- `ambiguous` — includes `targetRef` and `candidates`. Candidates are profile
  field keys, not raw values. Ask the user or leave unresolved; do not guess.
- `no_match` — includes `targetRef`, `reason`, and `message`. Ask for the
  missing open fact only when the field is required for the user's task.

Blocked shape:

```json
{
  "success": false,
  "outcomeType": "blocked",
  "error": "profile_facts_unavailable",
  "message": "MagicPay profile facts are unavailable.",
  "reason": "Could not fetch profile facts.",
  "nextAction": "ask-user"
}
```

`resolve-fields.error` values:

- `target_refs_required` — no target refs were supplied. Re-observe and pass
  explicit required target ids.
- `observed_target_not_found` — at least one target id is stale or absent.
  Re-observe and retry with fresh target ids.
- `profile_facts_unavailable` — profile facts could not be loaded. Fail
  closed or ask the user to retry later.
- `matcher_unavailable` — open-data matching is unavailable. Fail closed; do
  not invent values.
- `workflow_session_required` — there is no active MagicPay product workflow
  session. Start or restore the product session first.
- `data_request_failed` — the MagicPay missing-data request could not be
  created. Surface the blocker and ask the user how to proceed.

## `resolve-form`

Success shape:

```json
{
  "success": true,
  "ok": true,
  "requestId": "req_123",
  "resolutionPath": "auto",
  "itemRef": "vault_item_ref",
  "fill": {
    "outcome": "filled",
    "filledFields": [{ "fieldKey": "pan", "targetRef": "12" }]
  }
}
```

Branch on `success`, then `fill.outcome`:

- `filled` — protected values were filled. Refresh the browser state and
  continue with the browser owner. If the next browser action is
  consequential, get matching typed MagicPay approval.
- `form_changed` — the protected form changed after approval. Rerun
  `find-form` on the current page before requesting or filling again.

Failure shape:

```json
{
  "success": false,
  "ok": false,
  "requestId": "req_123",
  "reason": "failed",
  "error": "secret_validation_failed",
  "nextAction": "ask-user",
  "fieldErrors": []
}
```

Branch on `reason` and optional `error`:

- `denied`, `expired`, `failed`, `canceled`, `timeout` — stop the protected
  path and report the exact state.
- `error: "secret_validation_failed"` — client-side validation rejected one
  or more protected fields. Surface the blocker without exposing protected
  values and wait for user direction.
- `error: "protected_fill_blocked"` — the browser fill layer refused the
  protected fill. Treat as blocked; refresh state before any retry.

## Request Paths

- `auto` — MagicPay resolved the request without waiting for a new user
  decision.
- `confirm` — MagicPay paused for explicit approval before using the protected
  data or action path.
- `provide` — MagicPay paused because the user needed to provide missing data
  or select the right item.
- terminal `denied`, `expired`, `failed`, `canceled`, or `timeout` — stop the
  protected path and report the exact state.

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
