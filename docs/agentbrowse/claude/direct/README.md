# Install AgentBrowse in Claude Code

AgentBrowse lets Claude Code launch or attach to a browser, observe the visible page state, act on interactive targets, use MagicPay-backed goal observe and extract, and close the browser through the browser-only agentbrowse CLI.

## Before you start

- Node.js 18 or later is installed.
- Claude Code is installed and working.
- Goal-driven `observe` and `extract` require a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The environment allows browser launch or provides a CDP endpoint for attach.

## Ask your agent

Copy this request into your agent session:

```text
Install the `agentbrowse` skill in Claude Code from https://github.com/MercuryoAI/skills/releases/tag/agentbrowse-v0.1.6 using `agentbrowse-claude-code-v0.1.6.zip`. Place the extracted `agentbrowse/` folder directly inside `.claude/skills/` or `~/.claude/skills/`.
If I ask for goal-driven `observe` or any `extract`, ask me for my MagicPay API key and run `agentbrowse init <your-api-key>`.
If `agentbrowse` is missing, install or repair `@mercuryo-ai/agentbrowse-cli`.
Verify the setup with `agentbrowse --version`; use `agentbrowse doctor` only if init or local config still fails.
```

## What your agent should do

1. Download `agentbrowse-claude-code-v0.1.6.zip` from https://github.com/MercuryoAI/skills/releases/tag/agentbrowse-v0.1.6.
2. Extract the archive so the top-level folder is `agentbrowse/`.
3. Place `agentbrowse/` directly inside `.claude/skills/` or `~/.claude/skills/`.
4. If `agentbrowse` is missing, install or repair `@mercuryo-ai/agentbrowse-cli`.
5. Only if the task needs goal-driven `observe` or any `extract`, request your MagicPay API key and run `agentbrowse init <your-api-key>`.
6. Start a fresh Claude Code session if the current session does not pick up the installed skill.

Supported skill locations:

- `.claude/skills/`
- `~/.claude/skills/`

## Verify the result

1. Ask Claude Code to run `agentbrowse --version` and confirm the skill is available.
2. Ask Claude Code to use AgentBrowse for a small browser-only task.
3. If you need goal-driven `observe` or any `extract`, ask Claude Code to run `agentbrowse init <your-api-key>`; use `agentbrowse doctor` only if init or local config still fails.

## Try a first task

Start with a browser-only task. Only add `agentbrowse init <your-api-key>` when you need goal-driven `observe` or any `extract`.

- Use AgentBrowse to launch the browser, open this page, and summarize the visible pricing table.
- Use AgentBrowse to attach to the already running browser and click through the search form.
- Use AgentBrowse to extract the visible shipment options after running `agentbrowse init <your-api-key>` for goal-driven extract.

## Manual zip fallback

Use this fallback only if you want to manage the `agentbrowse/` folder yourself instead of asking Claude Code to install it from https://github.com/MercuryoAI/skills/releases/tag/agentbrowse-v0.1.6 using `agentbrowse-claude-code-v0.1.6.zip`.

1. Download `agentbrowse-claude-code-v0.1.6.zip`.
2. Extract the archive. You should get a folder named `agentbrowse/`.
3. Move that folder into one of the supported install locations below.
4. Restart Claude Code if you already had a session open.

## Update or reinstall

1. Ask your agent to download the newer archive for this Claude Code runtime and replace the existing skill folder with the new `agentbrowse/` folder.
2. Keep the folder name exactly `agentbrowse` inside the same skills directory.
3. If you rely on goal-driven `observe` or `extract` and the local config was reset, rerun `agentbrowse init <your-api-key>`.
4. Start a fresh Claude Code session after the replacement.

## Troubleshooting

- **Skill not recognized by Claude Code**: Make sure the folder sits directly inside `.claude/skills/` or `~/.claude/skills/`.
- **`agentbrowse` command not found**: Ask the agent to install or repair `@mercuryo-ai/agentbrowse-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/agentbrowse-cli`.
- **Goal-driven observe or extract still fails**: Ask the agent to run `agentbrowse init <your-api-key>` and use `agentbrowse doctor` only for local config diagnostics.
- **Browser launch or attach is unavailable**: Provide a live CDP endpoint or use an environment that allows browser startup.

---

This guide is for AgentBrowse v0.1.6.
