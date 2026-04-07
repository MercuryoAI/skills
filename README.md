# Mercuryo MagicPay Skills

Installable skill bundles for MagicPay browser and protected-form workflows in Claude Code, Codex, and OpenClaw.

## What is a skill?

A skill is an instruction bundle your AI runtime can load. The skill tells the agent when to use the CLI, what to ask you, and how to verify the result. The CLI does the actual browser or protected-form work.

## Choose a surface

| If you need this | Choose this surface | Why |
| --- | --- | --- |
| Browser-only navigation, observation, and extraction | `agentbrowse` | Use this when the agent only needs browser control. Goal-driven `observe` and `extract` can use your MagicPay API key later, but simple browser tasks do not need it. |
| A protected login, identity, or payment form is already open and ready | `magicpay` | Use this when another tool already prepared the page and you only need the protected-form flow. The normal path ends at `fill-secret`; `submit-form` is manual recovery only. |

## How to use this catalog

1. Pick the guide for your runtime below.
2. Open that guide and copy the request from the `Ask your agent` section into your agent session.
3. Let the agent install, initialize, and verify the skill.
4. Use the manual zip or folder steps only if your agent cannot complete the install itself.

## Start with the right guide

### `agentbrowse`

Browser-only operator skill for launch, attach, observe, act, extract, and navigation workflows.

- Claude Code: [direct guide](docs/agentbrowse/claude/direct/README.md)
- Codex: [direct guide](docs/agentbrowse/codex/direct/README.md)
- OpenClaw: [ClawHub guide](docs/agentbrowse/openclaw/marketplace/README.md) or [direct fallback](docs/agentbrowse/openclaw/direct/README.md)

### `magicpay`

Payment, session, and protected-secret skill for already prepared browser contexts.

- Claude Code: [direct guide](docs/magicpay/claude/direct/README.md)
- Codex: [direct guide](docs/magicpay/codex/direct/README.md)
- OpenClaw: [ClawHub guide](docs/magicpay/openclaw/marketplace/README.md) or [direct fallback](docs/magicpay/openclaw/direct/README.md)

## Layout

- `docs/` contains generated installation guides by skill and runtime.
- `manifests/` contains per-skill release metadata and owner/bin mappings.
- `surface-manifest.json` describes the exported public artifact set.
