# Mercuryo Agent Skills

Installable skill bundles for MagicBrowse LLM-first browser autonomy fallback and MagicPay protected-form workflows in Claude Code, Codex, and OpenClaw.

## What is a skill?

A skill is an instruction bundle your AI runtime can load. The skill tells the agent when to use the CLI, what to ask you, and how to verify the result. The CLI does the actual browser or protected-form work.

## Choose a surface

| If you need this | Choose this surface | Why |
| --- | --- | --- |
| Reach a target web page when your own browser tooling cannot do it reliably | `magicbrowse` | LLM-first browser autonomy fallback. Drive a Chrome session by natural-language goals through `magicbrowse act`; use deterministic `observe` plus primitives only when autonomy needs single-element precision. Stops at the boundary of any protected form. |
| A prepared login, identity, or payment form needs approved stored values | `magicpay` | Use this when the browser is already at the protected step and the remaining work is discovery, approval, fill, and guarded submit. |

## How to use this catalog

1. Pick the guide for your runtime below.
2. Open that guide and copy the request from the `Ask your agent` section into your agent session.
3. Let the agent install, initialize, and verify the skill.
4. Use the manual zip or folder steps only if your agent cannot complete the install itself.

## Start with the right guide

### `magicbrowse`

Browser automation fallback — drive an approved Chrome session by natural-language steps via launch, attach, observe, and act, with deterministic click/type/select/press/fill primitives.

- Claude Code: [direct guide](docs/magicbrowse/claude/direct/README.md)
- Codex: [direct guide](docs/magicbrowse/codex/direct/README.md)
- OpenClaw: [ClawHub guide](docs/magicbrowse/openclaw/marketplace/README.md) or [direct fallback](docs/magicbrowse/openclaw/direct/README.md)

### `magicpay`

Protected-form workflow skill for approved prepared login, identity, and payment pages with fill-for-review before submit.

- Claude Code: [direct guide](docs/magicpay/claude/direct/README.md)
- Codex: [direct guide](docs/magicpay/codex/direct/README.md)
- OpenClaw: [ClawHub guide](docs/magicpay/openclaw/marketplace/README.md) or [direct fallback](docs/magicpay/openclaw/direct/README.md)

## Layout

- `docs/` contains generated installation guides by skill and runtime.
- `manifests/` contains per-skill release metadata and owner/bin mappings.
- `surface-manifest.json` describes the exported public artifact set.
