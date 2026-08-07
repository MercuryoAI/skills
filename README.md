# Nuanu AI Skills

Installable skill bundles for MagicBrowse browser page-control fallback and MagicPay approved login, identity, checkout, donation, subscription, and payment pages in supported AI runtimes.

## What is a skill?

A skill is an instruction bundle your AI runtime can load. The skill tells the agent when to use the CLI, what to ask you, and how to verify the result. The CLI does the actual browser or MagicPay work.

## Choose a surface

| If you need this | Choose this surface | Why |
| --- | --- | --- |
| Reach a target web page when the runtime page-control tool cannot do it reliably | `magicbrowse` | LLM-first browser page-control fallback. Drive a Chrome session by natural-language goals through `magicbrowse act`; use deterministic `observe` plus primitives only when autonomy needs single-element precision. Stops at login, identity, checkout, donation, subscription, payment, and human-verification pages. |
| A login, identity, checkout, donation, subscription, or payment workflow needs approved stored values | `magicpay` | Start the MagicPay product session first, then launch or attach the browser as a child resource for discovery, approval, protected fill, and browser-owner continuation. |

## How to use this catalog

1. Pick the guide for your runtime below.
2. Open that guide and copy the request from the `Ask your agent` section into your agent session.
3. Let the agent install, initialize, and verify the skill.
4. Use the manual zip or folder steps only if your agent cannot complete the install itself.

## Publication surfaces

This repository is generated from the private release source. Use `manifests/<skill>.json` as the machine-readable source of truth for archive names, runtime docs, npm owner packages, and marketplace payload paths.

| Surface | Public location | Use it for |
| --- | --- | --- |
| GitHub Releases | Release assets on `magicbrowse-v*` and `magicpay-v*` tags | Direct zip installs for runtimes that load local skill folders. Use the runtime direct guide for the exact archive name and install path. |
| Runtime install guides | `docs/<skill>/<runtime>/{direct,marketplace}/README.md` | Copy/paste prompts for your agent, including init, verification, and manual fallback steps. |
| OpenClaw ClawHub | ClawHub plus `docs/<skill>/openclaw/marketplace/README.md` | Preferred OpenClaw install path; direct zip remains a fallback. |
| Hermes GitHub hub/tap | `hermes/<skill>/SKILL.md` | Add the `nuanu-ai/skills` tap in Hermes, then install the skill from `nuanu-ai/skills/hermes/<skill>`. |
| skills.sh / universal agents | `skills/<skill>/SKILL.md` in this repository | Install with `npx skills add nuanu-ai/skills --skill <skill> --yes --copy`; skills.sh URLs stay under `nuanu-ai/skills/<skill>`. |

## Start with the right guide

### `magicbrowse`

Browser page-control fallback — drive an approved Chrome session with goal-driven act as the default MagicBrowse primitive; use observe plus deterministic click/type/select/press/fill only for recovery or single-element precision, then verify any expected page-state change with a fresh observation.

- Claude Code: [direct guide](docs/magicbrowse/claude/direct/README.md)
- Codex: [direct guide](docs/magicbrowse/codex/direct/README.md)
- OpenClaw: [ClawHub guide](docs/magicbrowse/openclaw/marketplace/README.md) or [direct fallback](docs/magicbrowse/openclaw/direct/README.md)

### `magicpay`

MagicPay helper for approved login, identity, checkout, donation, subscription, payment pages, and typed action approvals.

- Claude Code: [direct guide](docs/magicpay/claude/direct/README.md)
- Codex: [direct guide](docs/magicpay/codex/direct/README.md)
- Hermes: [Hub/tap guide](docs/magicpay/hermes/marketplace/README.md) or [direct fallback](docs/magicpay/hermes/direct/README.md)
- OpenClaw: [ClawHub guide](docs/magicpay/openclaw/marketplace/README.md) or [direct fallback](docs/magicpay/openclaw/direct/README.md)
- General Agent: [skills.sh guide](docs/magicpay/skills-sh/marketplace/README.md) or [direct fallback](docs/magicpay/skills-sh/direct/README.md)

## Layout

- `docs/` contains generated installation guides by skill and runtime.
- `skills/` contains universal skills.sh payloads.
- `hermes/` contains Hermes GitHub hub/tap payloads.
- `manifests/` contains per-skill release metadata and owner/bin mappings.
- `skills.sh.json` customizes the skills.sh repository page.
- `surface-manifest.json` describes the exported public artifact set.
