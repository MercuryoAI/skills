# Memory

Use this reference for saved Memory items, their field definitions, and any
request that would collect or use a stored value. MCP owns atomic value-free
operations. The skill owns scope choice, safe sequencing, and exact
continuation.

## Scope and reads

- Choose exactly one of a URL scope or all-sites scope for
  `list_memory_items` and `show_memory_items`. Preserve an explicit status
  filter. Do not silently widen a URL lookup to all sites.
- Use `show_memory_items` only when the user directly asks to see Memory.
  Otherwise keep reads silent with `list_memory_items` or `get_memory_item`.
- Treat item IDs, `fieldRef` handles, and content revisions as opaque exact
  references. Do not rebuild or guess them from labels.

## Value-free CRUD

- Create only typed item and field metadata with `create_memory_item`. Read the
  returned item before a later mutation if its current content revision is not
  already authoritative.
- Patch item metadata with `update_memory_item`; soft-delete an exact item with
  `delete_memory_item`.
- Add a field with `add_memory_field`. Update or delete only an exact `fieldRef`
  with `update_memory_field` or `delete_memory_field`.
- Supply the exact current content revision whenever the tool requires it. On
  a revision mismatch, read the same item again, re-evaluate the requested
  change, and retry only that change. Never overwrite concurrent edits.
- Stop on an unknown or ambiguous `fieldRef`, or a read-only item. Ask the user
  to identify the intended item when the exact target cannot be resolved.

## Values and secure continuation

Never put a stored Memory field value in chat, an MCP argument, a tool log, or
normal conversation. The narrow exception is eligible ordinary output from
`resolve_browser_form_values`: its structured content may contain values for
normal Browser filling in the exact session and page. Do not repeat those
values in prose. The direct browser payment run may likewise return approved
ordinary checkout values. Passwords, OTPs, API keys, wallet secrets, private
keys, seeds, payment-card values, and provider-managed values never use either
model-visible path.

When a value is supplied to a value-free mutation, the MCP returns the
secure_collection_required continuation with safe field metadata. Preserve
that exact operation and item identity, tell the user that secure collection is
required, and call `request_memory_values` with field metadata only. Give the
user its exact request URL, then call `wait_memory_request` on the same request
ID. Never ask for the value in chat and never invent a request URL or request
ID.

Use `correct_memory_field` only after the host has selected the exact live
target. Pass the returned `fieldRef` and host target ID; give the user its exact
secure request URL and wait with `wait_memory_request`. After fulfillment, use
only the returned value-free item and its new content revision. Denied,
expired, failed, or canceled is terminal for that request.

For choice or ordinary reply requests that already exist, use the exact generic
request loop in [requests.md](requests.md). A choice or reply is authorization
for that exact request only; it does not reveal or authorize reuse of a stored
value.

## General browser forms

For an ordinary non-payment form, inspect the rendered page and call
`get_memory_catalog`, then `resolve_browser_form_values` with generic field
descriptions and value-free matches. Do this before offering manual page entry.

- `ready`: fill the returned `model_visible_form` values through the normal
  Browser, then re-observe once. Do not quote the values or submit the form.
- `request_required`: ask for the returned fields or choice in chat and decide
  that exact request. For a value request, append this exact sentence:

  > If you'd like me to save these values for later, say “save this for later”
  > and briefly describe what they are for.

- `fallback_required`: explain the reason briefly and offer manual page entry.

Saving is off unless the user explicitly says to save and supplies a semantic
description. If they ask to save without a description, ask one short follow-up
before deciding the request. Convert their description into one concise Memory
`displayName`; do not forward the raw description, invent meaning, or add
field-level hints. Submit `saveForFuture: true` with
`saveAs: { displayName }`. An explicit no-save instruction wins.

After the decision, notify the user only from `saveOutcome`. Say whether a
Memory item was saved or updated, using its returned display name, site scope,
and field labels without repeating values. `unchanged` means current-run use
only. If the decision fails, say the values were not saved. Do not add another
read, save attempt, or notification system.

The resolver may use an existing claim-once current-run artifact. If that
artifact is unavailable, accept `fallback_required`; do not create replay state
or a sibling request. Browser owns selectors, control behavior, rerenders, and
the page itself. The resolver never navigates or submits.

## Catalog, choice, and direct browser checkout

For agent-direct checkout, send semantic `ordinaryFieldRoles` in the composed
run. It tries saved Memory first. If it returns `ordinary_field_required`, use
the exact run-owned request: the user may reply with those ordinary values in
regular chat for `decide_request`, or use the returned Memory request. Continue
the same run; do not create `request_memory_values`, another payment run, or a
replacement checkout. Chat-provided values remain current-run data unless the
user explicitly chooses to save them.

Do not route checkout email or other ordinary checkout values through a sealed
fill lane. Direct browser payment resolves them in the composed run and falls
back to the exact ordinary chat request when Memory is unavailable.
