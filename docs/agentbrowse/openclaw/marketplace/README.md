# Install AgentBrowse from ClawHub

AgentBrowse gives OpenClaw a browser automation layer for launching or attaching to a browser, reading the current page, acting on visible controls, navigating directly, extracting structured data, and capturing screenshots through the `agentbrowse` CLI.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- The environment can launch a browser or provides a reachable CDP endpoint for attach.
- If you plan to use goal-based `observe` or `extract`, have your API key ready. Sign up at https://agents.mercuryo.io/signup if needed.

## Ask your agent

Copy this request into your agent session:

```text
Install the `agentbrowse` skill from ClawHub in this workspace.
Treat AgentBrowse as the browser layer for launch, attach, page observation, actions, navigation, extract, and screenshots.
If I ask for goal-based `observe` or any `extract`, ask me for my API key and run `agentbrowse init <your-api-key>`.
If `agentbrowse` is missing, install or repair `@mercuryo-ai/agentbrowse-cli`.
Verify the setup by using AgentBrowse on a small browser task. Use `agentbrowse doctor` only if semantic commands still fail after init.
```

## Install source

- Preferred path: ask your agent to install the `agentbrowse` skill from ClawHub in the current workspace.
- If the task needs goal-based `observe` or `extract`, ask the agent to request your API key and run `agentbrowse init <your-api-key>`.
- ClawHub slug: `agentbrowse`

## What your agent should do

1. Install the `agentbrowse` skill from ClawHub in the current workspace.
2. Only if the task needs goal-based `observe` or any `extract`, request your API key and run `agentbrowse init <your-api-key>`.
3. Verify the install by using AgentBrowse on a small browser task.
4. Start a fresh OpenClaw session if the current session does not see the installed skill.

## Verify the result

1. Ask OpenClaw to use AgentBrowse for a small browser task such as launch, attach, or observe.
2. If you need goal-based `observe` or any `extract`, ask OpenClaw to run `agentbrowse init <your-api-key>` and then retry a small semantic task.
3. If semantic commands still fail after init, use `agentbrowse doctor` to inspect the local config.

## Try a first task

Start with a real browser task: open or attach to a page, inspect the visible state, and act from there. Add `agentbrowse init <your-api-key>` only when you want goal-based `observe` or structured `extract`.

- Use AgentBrowse to launch the browser, open this page, and summarize the visible pricing table.
- Use AgentBrowse to attach to the already running browser and continue the search flow from the current page.
- Use AgentBrowse to extract the visible shipment options after running `agentbrowse init <your-api-key>` for semantic extract.

## Manual zip fallback

Use the manual path only if you want to manage the skill files yourself instead of letting OpenClaw install from ClawHub.

1. Download the public release archive for the current AgentBrowse skill version.
2. Extract the archive. You should get a folder named `agentbrowse/`.
3. Move that folder into `skills/` or `~/.openclaw/skills/`.
4. Restart OpenClaw if you already had a session open.

## Update or reinstall

1. Ask your agent to update the `agentbrowse` skill from ClawHub.
2. If you rely on goal-based `observe` or `extract` and the local config was reset, rerun `agentbrowse init <your-api-key>`.
3. Start a fresh OpenClaw session if the current session still does not see the updated skill.

## Troubleshooting

- **Skill not recognized by OpenClaw**: Start a fresh OpenClaw session so the runtime can rescan installed skills.
- **`agentbrowse` command not found**: Ask the agent to install or repair `@mercuryo-ai/agentbrowse-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/agentbrowse-cli`.
- **Semantic observe or extract needs API access**: Ask the agent to run `agentbrowse init <your-api-key>` or provide `MAGICPAY_API_KEY` before retrying.
- **Semantic commands still fail after init**: Ask the agent to run `agentbrowse doctor` and inspect the local config.
- **Browser launch or attach is unavailable**: Provide a live CDP endpoint or use an environment that allows browser startup.
- **The task reaches a protected login or payment step**: Keep AgentBrowse for browser control and switch the protected step to MagicPay or MagicPay Agent.

---

This guide is for AgentBrowse v0.1.14.
