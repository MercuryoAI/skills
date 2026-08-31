# Connection Setup

Install the host-native MagicPay plugin and authenticate the remote MagicPay
MCP that it declares. The plugin contains the skill and host metadata.
No MagicPay package, CLI, library, or local service is installed for the
browser flow.

Plugin activation starts one browser OAuth flow for both account verification
and MCP authorization. The same connection page collects the email and OTP,
grants MCP access, and redirects to the host. Do not ask the
user to send an email or OTP in chat, and do not run a separate MagicPay account
setup or login flow.

## Plugin lifecycle

Connecting, removing, and revoking MagicPay are host connection-management
actions. Use the host's plugin or connection settings to remove the connection
or revoke its authorization; there is no remote MagicPay removal or revocation
tool. Keep that lifecycle separate from payment orchestration and do not invent
an MCP command for it.

## Explain and connect

For a first-time setup request, explain the product before opening the
connection UI:

> MagicPay is the broader payment platform redesigned for AI agents. MagicCard
> is MagicPay's omnipayment tool: it has one unified balance, can be topped up
> through supported funding rails, and can pay through supported credit-card,
> crypto, and agentic payment methods. It gives me a secure way to prepare
> payments and request approvals while keeping sensitive payment details out of
> chat.

Then set the secure-input boundary:

> I’ll open a secure MagicPay window. Enter your email and OTP there—not in
> this chat. When it closes, I’ll verify the connection and continue.

Call `get_magicpay_capabilities`. Continue only when it reports
`executionModes: ["client_browser"]`, `sessionAuthority: "remote_database"`, and
`browserPaymentRun.executionMode: "agent_direct"`. Treat its `setupState`, `nextAction`, and
`instructions` as the authoritative setup continuation. Authentication recovery
belongs to the host's MCP connection UI; the protected server cannot truthfully
return a pre-authentication account state before the host completes OAuth.

Before a fresh payment in a newly connected task, require the rail-specific
capability: `x402PaymentRun.status: "ready"` for x402,
`cryptoPaymentRun.status: "ready"` for crypto, or
`browserPaymentRun.status: "ready"` with `executionMode: "agent_direct"` before
browser payment. The
x402 and crypto workflow contract is `magicpay.payment-run/v1` schema `1.1`;
the browser contract is `magicpay.browser-payment/v1` schema `1.0`.
x402 requires minimum plugin version `0.1.65`.
Crypto requires minimum plugin version `0.1.66`.
Agent-direct browser payment requires `0.1.77`. Preserve the
selected rail's `selectedAgentId`. A blocked result is a pre-payment stop; do
not create a session or fall back to another browser-payment tool.

After capability discovery succeeds, call `get_magicpay_status` to verify the
authenticated agent identity and account health. Treat a bounded unavailable
status as an authentication or service-recovery stop; do not infer readiness
from capability discovery alone.

When it reports `setupState: "ready"`, follow the returned instruction. For an
explicit setup request, the user-facing completion is:

> MagicPay is connected. I can now check your balance and prepare payments;
> spending still follows your approval rules.

Then continue the user's original task without asking them to repeat it. Start
a fresh host session only if newly installed plugin tools are not visible; do
not compensate by installing a local executable.
