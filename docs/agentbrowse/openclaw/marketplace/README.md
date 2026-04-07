# Install AgentBrowse from ClawHub

AgentBrowse lets OpenClaw launch or attach to a browser, observe the visible page state, act on interactive targets, use MagicPay-backed goal observe and extract, and close the browser through the browser-only agentbrowse CLI.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- Goal-driven `observe` and `extract` require a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The environment allows browser launch or provides a CDP endpoint for attach.

## Ask your agent

Copy this request into your agent session:

```text
Install the `agentbrowse` skill from ClawHub in this workspace.
If I ask for goal-driven `observe` or any `extract`, ask me for my MagicPay API key and run `agentbrowse init <your-api-key>`.
If `agentbrowse` is missing, install or repair `@mercuryo-ai/agentbrowse-cli`.
Verify the setup with `agentbrowse --version`; use `agentbrowse doctor` only if init or local config still fails.
```

## Install source

- Preferred path: ask your agent to install the `agentbrowse` skill from ClawHub in the current workspace.
- If the task needs goal-driven `observe` or `extract`, ask the agent to request your MagicPay API key and run `agentbrowse init <your-api-key>`.
- ClawHub slug: `agentbrowse`

## What your agent should do

1. Install the `agentbrowse` skill from ClawHub in the current workspace.
2. Only if the task needs goal-driven `observe` or any `extract`, request your MagicPay API key and run `agentbrowse init <your-api-key>`.
3. Confirm the install by running `agentbrowse --version`.
4. Start a fresh OpenClaw session if the current session does not see the installed skill.

## Verify the result

1. Ask OpenClaw to run `agentbrowse --version` and confirm the skill is available.
2. Ask OpenClaw to use AgentBrowse for a small browser-only task.
3. If you need goal-driven `observe` or any `extract`, ask OpenClaw to run `agentbrowse init <your-api-key>`; use `agentbrowse doctor` only if init or local config still fails.

## Try a first task

Start with a browser-only task. Only add `agentbrowse init <your-api-key>` when you need goal-driven `observe` or any `extract`.

- Use AgentBrowse to launch the browser, open this page, and summarize the visible pricing table.
- Use AgentBrowse to attach to the already running browser and click through the search form.
- Use AgentBrowse to extract the visible shipment options after running `agentbrowse init <your-api-key>` for goal-driven extract.

## Manual zip fallback

Use the manual path only if you want to manage the skill files yourself instead of letting OpenClaw install from ClawHub.

1. Download the public release archive for the current AgentBrowse skill version.
2. Extract the archive. You should get a folder named `agentbrowse/`.
3. Move that folder into `skills/` or `~/.openclaw/skills/`.
4. Restart OpenClaw if you already had a session open.

## Update or reinstall

1. Ask your agent to update the `agentbrowse` skill from ClawHub.
2. If you rely on goal-driven `observe` or `extract` and the local config was reset, rerun `agentbrowse init <your-api-key>`.
3. Start a fresh OpenClaw session if the current session still does not see the updated skill.

## Troubleshooting

- **Skill not recognized by OpenClaw**: Start a fresh OpenClaw session so the runtime can rescan installed skills.
- **`agentbrowse` command not found**: Ask the agent to install or repair `@mercuryo-ai/agentbrowse-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/agentbrowse-cli`.
- **Goal-driven observe or extract still fails**: Ask the agent to run `agentbrowse init <your-api-key>` and use `agentbrowse doctor` only for local config diagnostics.
- **Browser launch or attach is unavailable**: Provide a live CDP endpoint or use an environment that allows browser startup.

---

This guide is for AgentBrowse v0.1.5.
