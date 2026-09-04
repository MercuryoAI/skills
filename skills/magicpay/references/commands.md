# MCP Routing Map

MCP discovery is authoritative for exact arguments and results. This reference
only distinguishes starting routes, continuations, and explicit presentation.
Use opaque IDs exactly as returned.

## Starting routes

| User intent | Start here |
| --- | --- |
| Connect or recover | `get_magicpay_capabilities`, then `get_magicpay_status` |
| Account readiness | `account_status` |
| Recent user activity | `list_recent_transactions` |
| Balance | `get_payment_balance` |
| Generic top-up | `show_topup` |
| Funding methods, link, or addresses | Use the exact funding action requested |
| New crypto transfer | `run_crypto_transfer` |
| Known raw x402 resource | `run_x402_payment` |
| Known checkout destination | `create_checkout_session` |
| Unknown product or provider | `create_purchase_intent` |
| Selected paid x402 discovery | `execute_commerce_option` with durable `runId`, `runRevision`, `clientRequestId`, and `type` |
| Existing payment status | `get_payment_operation` |
| Manage saved Memory | Use the direct value-free CRUD action matching the intent |
| Use Memory in an active session | `get_memory_footprint`, then exact `materialize_memory_items` or v3 `resolve_browser_form_values` |
| Existing request, session, or operation | Use only its returned `nextAction` |

Do not use autonomous `start_session` for a normal host-browser checkout. A
request that already names a merchant, recipient, or site does not require
provider discovery: find its official destination in the host browser, then
create the checkout session.

## Silent work and presentation

Operational reads, mutations, approval, waits, status checks, and reconciliation
stay silent inside a broader task. Open a widget only for the current user's
explicit request to see that exact surface:

- `show_account_status`
- `show_memory_items`
- `show_payment_balance`
- `show_payment_operation`
- `show_topup`
- `show_session`
- `show_session_request`
- `show_sessions`
- `show_subscriptions`

The only automatic presentation is `show_topup` when the authoritative unified
balance returns `funding_required`. Service, policy, approval, card-pool, and
ambiguous-submission errors are not funding requests.

## Exact continuations

- Continue a composed payment with `wait_payment` using the same `runId` and
  progress cursor.
- Read one operation with `get_payment_operation`; call
  `reconcile_payment_operation` only for that exact operation and only when its
  state directs reconciliation.
- Continue a runtime request with `get_request`, `wait_request`,
  `decide_request`, or `confirm_request_otp` using its exact request identity.
- Continue a checkout with its exact session lifecycle tools. Cancellation or
  failure closure never proves settlement.
- Continue unknown-provider discovery through its exact purchase-intent and run
  identities. A selected paid x402 method continues only with
  `execute_commerce_option` and its durable run/revision/key/type; browser
  methods use the returned checkout continuation. Never derive a raw request,
  debit cap, or private selection handle from a guide or discovery output.
- Continue a Memory choice with `decide_request` using `choose_candidate` and
  its exact `selectedChoiceId`. Continue approval or collection through the
  exact returned request, then re-run the same materialization/resolver call.

## Compatibility and internal tools

Fresh transfers and x402 payments use the composed run tools. Lower-level
`start_direct_transfer`, `start_x402_purchase`, and
`get_x402_purchase_result` remain available only for an already-existing legacy
operation; do not use them to compose new work.

Historical browser-reconciliation tools may inspect or close an already-created
historical attempt. They cannot start or continue a new browser payment.

Widget-internal tools are app-only in MCP visibility. Development tools remain
behind `DEV_MODE`. Neither class is a model route. Public payment instruments
are internal infrastructure: inventory, provider-card details, compatibility
balances, and provider-card history remain intentionally unavailable.
Route every user balance request through the unified payment-balance tools.

General help and hosted autonomous-session tools remain compatibility routes for
supported older skills. Prefer the operational starting route above when it can
answer the user's current intent.
