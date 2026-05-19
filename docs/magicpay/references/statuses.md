# MagicPay Resolution And Action States

## Protected-Form Discovery

- `form_found`
  A supported protected form is available on the current page.
- `protected_form_not_found`
  The current page does not expose a supported protected form. Verify the page
  context before retrying.
- `protected_form_ambiguous`
  Several supported forms match. Surface the ambiguity and ask the user to
  choose.

## Request Paths

- `auto`
  MagicPay resolved the request without waiting for a new user decision.
- `confirm`
  MagicPay paused for explicit approval before using the protected data or
  action path.
- `provide`
  MagicPay paused because the user needed to provide missing data or select the
  right item.
- terminal `denied`, `expired`, `failed`, `canceled`, or `timeout`
  Stop the protected path and report the exact state.

### `session_stop`

A special variant of `canceled`: the whole workflow session was terminated
mid-flow by the user, a trust rule, or the backend. The result includes
`session_stop` details with a `code` and a human-readable `message`.

Do not retry the same request inside the same session. End the session
with `magicpay end-session`, then start a new one if the user wants to
continue.

## Fill Results

- `filled`
  Protected values were filled. Continue with the browser owner. If the next
  browser action is consequential, get the matching typed MagicPay approval.
- `form_changed`
  The protected form changed after approval, so the filled request could not
  be applied to the original form contract. Rerun `find-form` on the current
  page before requesting new protected values.
- `secret_validation_failed`
  Client-side validation rejected the protected values. Surface the blocker
  without exposing protected values and wait for user direction.

## Protected Actions

- `artifact`
  A typed action command completed and returned the request artifact. Proceed
  with exactly that approved action; stop only if page facts changed.
- `pending`
  A typed action command with `--return-pending` created the request and
  stored `currentRequestId`. The user can approve in MagicPay UI or provide
  the OTP they received. If they approve in UI, run `wait-request`; if they
  provide OTP, run `confirm-otp --otp <digits>` and then `wait-request`.
- `otp_invalid`, `otp_expired`, `otp_attempts_exceeded`
  The OTP channel failed. Report the typed failure without repeating the OTP.
  While the request itself is still pending, keep MagicPay UI approval
  available.
- `request_already_resolved`
  Another channel already resolved the same pending request. Continue through
  `wait-request` or the returned terminal request state instead of applying a
  second decision.
