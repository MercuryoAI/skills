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
normal conversation. The direct browser run may return approved ordinary
checkout values such as email, name, address, country, phone, or postal code.
When an ordinary value is unavailable, the same run may request it in regular
chat. Passwords, OTPs, API keys, wallet secrets, private keys, seeds, and
payment-card values never use that fallback.

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
