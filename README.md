# Mercuryo MagicPay Skills

Installable skill bundles for Mercuryo browser and payment workflows in Codex, Claude Code, and OpenClaw.

## Available Skills

- `agentbrowse` — Browser-only operator skill for launch, attach, observe, act, extract, and navigation workflows.
- `magicpay` — Payment, session, and protected-secret skill for already prepared browser contexts.

## How To Use This Repo

### OpenClaw

1. Open `docs/<skill>/openclaw/marketplace/README.md` for the skill you want.
2. Ask your agent to install that skill from ClawHub in the current workspace.
3. If the guide says the skill needs setup, ask the agent to request your API key and run `init`.
4. Use the manual zip fallback from the same guide only if you want to manage the files yourself.

### Codex And Claude Code

1. Open the release for the skill you want to install.
2. Download the runtime-specific `.zip` bundle from that release.
3. Open the matching install guide under `docs/<skill>/<runtime>/direct/README.md`.
4. Extract the `<skill>/` folder into your runtime skill directory and start a fresh session.

## Layout

- `docs/` contains generated user-facing installation guides grouped by skill and runtime.
- `manifests/` contains per-skill release metadata and owner/bin mappings.
- `surface-manifest.json` describes the exported public-facing artifact set.
