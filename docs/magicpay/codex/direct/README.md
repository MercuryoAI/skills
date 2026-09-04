# Install MagicPay for Codex

Download the release archive and place the magicpay folder in a supported skill directory.

## Host plugin (recommended)

The host plugin bundles this skill and the remote MCP declaration, so no archive download is needed.

- Development channel: `codex plugin marketplace add nuanu-ai/skills --ref staging`, then `codex plugin add magicpay@nuanu-skills-staging`.
- Production channel, after promotion: the `nuanu-ai/skills` marketplace at its stable ref, then `codex plugin add magicpay@nuanu-skills`.
- Connect through the plugin Connect action or `codex mcp login magicpay`, then start a new task to load the tools.

## Direct skill archive

1. Install the MagicPay skill.
2. Connect the remote MagicPay MCP for this runtime.
3. Confirm get_magicpay_capabilities reports client_browser mode.

Supported skill directories:

- .codex/skills/
- ~/.codex/skills/

## Browser architecture

The runtime built-in browser is the only page-control owner. It navigates,
analyzes, fills ordinary fields, chooses exact protected targets, and
interprets results, and owns the one final commitment. Do not install or
start a second browser controller.

Release: magicpay-v0.1.105
