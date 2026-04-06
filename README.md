# Mercuryo MagicPay Skills

Installable skill bundles for Mercuryo browser and payment workflows in Codex, Claude Code, and OpenClaw.

## Available Skills

- `agentbrowse` — Browser-only operator skill for launch, attach, observe, act, extract, and navigation workflows.
- `magicpay` — Payment, session, and protected-secret skill for already prepared browser contexts.

## How To Use This Repo

1. Open the release for the skill you want to install.
2. Download the runtime-specific `.zip` bundle from that release.
3. Open the matching install guide under `docs/<skill>/<runtime>/direct/README.md`.
4. Extract the `<skill>/` folder into your runtime skill directory and start a fresh session.

## Layout

- `docs/` contains generated user-facing installation guides grouped by skill and runtime.
- `manifests/` contains per-skill release metadata and owner/bin mappings.
- `surface-manifest.json` describes the exported public-facing artifact set.
