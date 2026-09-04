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
- Use `request_choice` once when the user must select one of a few closed-world
  options. Preserve its exact `sessionId`, `requestId`, option order, and opaque
  option IDs. The selection authorizes only which option to continue with; it
  does not execute, book, purchase, approve payment, or prove settlement.
- Native transfer and x402 operations already return their operation-owned
  request UUID. Their payment decision belongs to the MagicPay approval system:
  read and wait on that exact request, and never ask for a chat confirmation or
  turn a plain chat “confirm” into `decide_request`. Never call
  `request_action_confirmation` or any standalone browser approval to replace a
  native approval.

## One choice request, two presentations

`request_choice` stores one durable request. Existing MagicPay request surfaces
present it, while a new `waiting_user` request returns the same stored options as
one numbered `structuredContent.chatMessage`. By default, echo that `chatMessage`
once as the assistant reply and yield. A runtime adapter may explicitly replace
that echo with one host-native single-select picker built only from the returned
stored `request.spec.prompt` and `request.spec.options`. The picker is merely the
chat presentation of the same durable MagicPay request: never show both it and
`chatMessage`, never create a sibling request, and never treat the host picker as
the decision authority. If the adapter cannot represent the exact stored option
set, fall back to the unchanged `chatMessage`. Do not copy the numbered list into
a separate tool-call message or compose another list from caller-local options.
Choose that presentation mode before showing any option. Once a native picker
has been shown for an active request, keep that same native presentation until
the user makes a valid stored selection, explicitly cancels or denies the
choice, or the request becomes terminal. Submit an explicit cancel or denial as
`decision: 'denied'` on the same request and then call `wait_request`; any other
unmapped, free-text, or host-added answer must be corrected through the same
picker and must never switch to `chatMessage`.
If an idempotent replay finds the exact request already terminal, `chatMessage`
is omitted: do not present the choices again; claim that same request with
`wait_request` and continue from its stored artifact.

On the next turn, map only an in-range ordinal, the exact opaque ID, or one
unambiguous exact title to the stored option ID. Submit an explicit cancel or
denial as `decision: 'denied'` on the same request and then call `wait_request`.
For any other or ambiguous reply, do not mutate the request and do not create a
sibling; ask the user to answer with a number or exact title from the same list.
Submit the matched ID to the same request, then call `wait_request` with the same
IDs to claim its stored artifact and continue once:

```text
decide_request({
  sessionId,
  requestId,
  decision: 'confirmed',
  selectedChoiceId
})
```

The request surface and chat race on that one request. The first durable
decision wins; reconcile any late or transport-ambiguous result against the same
request instead of overwriting it or creating another request. This echo and
yield loop is the deliberate choice exception to the immediate waiting rule
below.

## Present and wait

Report a returned `request_url` in normal conversation when review is needed.
When `otp_available` is true, explicitly tell the user they may send the fresh
six-digit approval OTP in chat for this exact request. When it is false or
absent, do not mention or request an OTP. Use a presentation widget only when
the user directly asks to see the exact request. For non-choice requests,
immediately call `wait_request` with the same IDs. If the bounded wait returns pending or aborted, call
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
- For a generic choice created by `request_choice`, submit `confirmed` with the
  exact `selectedChoiceId` as described above. Never infer or rewrite it.
- A Memory candidate-conflict request also uses the exact opaque
  `selectedChoiceId`, but its decision is `choose_candidate`. Never substitute
  an item ID or infer a candidate from labels.
- Submit `provided` values only for ordinary fields the user safely supplied in
  chat. Passwords, protected payment fields, private keys, seeds, and other
  protected values must use the secure request surface, not `values`.
- For a typed Memory collection, saving stays off unless the user's reply to
  that exact request includes **Save** and describes what the values are and
  when to reuse them. Submit `save: true` with one typed `saveGroups` entry per
  group, including its exact `groupRef`, template version, field mappings,
  `templateKey`, `displayLabel`, and semantic `description`. Omit `entity` to
  use the default Me entity; include an exact existing or explicit new entity
  only when the user identified it. Report persistence only from the returned
  `saveOutcome`, and never repeat submitted values.
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
