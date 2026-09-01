# Exact Runtime Requests

Use this loop for confirmation, message signing, opaque choices, ordinary data
replies, OTP, and operation-owned payment approvals. A request is always bound
to one exact remote `sessionId` and `requestId`; never create a sibling request
because a response, link, or waiter output was lost.

## Create

- Use `request_action_confirmation` for one clearly described consequential
  action. Preserve its exact summary and details. Fulfillment authorizes only
  that action; it does not execute it.
- Use `request_message_signature` for one exact wallet `itemRef` and the
  byte-for-byte message. Preserve whitespace. Never request or expose a private
  key, seed, or mnemonic.
- Native transfer and x402 operations already return their operation-owned
  request UUID. Their payment decision belongs to the MagicPay approval system:
  read and wait on that exact request, and never ask for a chat confirmation or
  turn a plain chat “confirm” into `decide_request`. Never call
  `request_action_confirmation` or any standalone browser approval to replace a
  native approval.

## Present and wait

Report a returned `request_url` in normal conversation when review is needed.
When `otp_available` is true, explicitly tell the user they may send the fresh
six-digit approval OTP in chat for this exact request. When it is false or
absent, do not mention or request an OTP. Use a presentation widget only when
the user directly asks to see the exact request. Immediately call `wait_request`
with the same IDs. If the bounded wait returns pending or aborted, call
`wait_request` again immediately in the same turn with those exact IDs; do not
return control to the user while the approval is still active. Use
`get_request` only to recover a lost snapshot or its exact hosted review link.

Approval, confirmation, choice, OTP, and signature creation are authority or
artifacts, not settlement and not proof that the requested external action ran.

## Decide

- Operation-owned payment approvals are decided in the MagicPay approval
  surface. The agent waits for that exact request; a chat response is not the
  authority to submit `confirmed` or `denied`.
- Submit `confirmed` or `denied` only from the user's explicit decision.
- Submit the backend's candidate-choice decision with the exact opaque choice ID returned by the
  backend. Never infer or rewrite it.
- Submit `provided` values only for ordinary fields the user safely supplied in
  chat. Passwords, protected payment fields, private keys, seeds, and other
  protected values must use the secure request surface, not `values`.
- For a general browser-form value request, saving stays off unless the user
  explicitly asks to save and describes the intended reuse. Polish that
  description into one `saveAs.displayName`, submit it with
  `saveForFuture: true`, and report persistence only from the returned
  `saveOutcome`. Never forward the raw description or repeat submitted values.
- A MagicPay approval OTP is intentionally accepted in chat only when the exact
  request says `otp_available: true`. Submit that fresh six-digit OTP only with
  `confirm_request_otp` on the same request; never place it in `values` or reuse
  it. OAuth and third-party OTPs remain outside chat.

On fulfilled, resume only the exact backend continuation. On denied, expired,
failed, canceled, session stop, identity mismatch, or protected-value request,
stop or follow the exact recovery returned by the backend. A signing result may
contain a signature artifact, but never private key material.

Browser payment approval and waiting belong only to `run_browser_payment` and
`wait_payment`; follow [host-browser-payments.md](host-browser-payments.md).
