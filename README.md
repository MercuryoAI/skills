# Agent Skills Public Surface

This directory is a public-ready export staging area generated from the canonical agent-skills catalog.

## Layout

- `releases/` contains versioned direct-install `.zip` archives for all first-wave skills.
- `docs/` contains generated user-facing installation guides grouped by skill and runtime.
- `manifests/` contains per-skill release metadata and owner/bin mappings.
- `surface-manifest.json` describes the exported public-facing artifact set.

## Source

- Generated from `packages/agent-skills/dist/**`.
- Publish direct downloads from this exported surface, not from the private monorepo directly.
