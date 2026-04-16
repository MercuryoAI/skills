# Install AgentBrowse in OpenClaw

AgentBrowse gives OpenClaw a browser automation layer for launching or attaching to a browser, reading the current page, acting on visible controls, navigating directly, extracting structured data, and capturing screenshots through the `agentbrowse` CLI.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- The environment can launch a browser or provides a reachable CDP endpoint for attach.
- If you plan to use AI-assisted `observe` (with a natural-language goal) or any `extract`, have your API key ready. Sign up at https://agents.mercuryo.io/signup if needed.

## Ask your agent

Copy this request into your agent session:

```text
Install the `agentbrowse` skill in OpenClaw from https://github.com/MercuryoAI/skills/releases/tag/agentbrowse-v0.1.22 using `agentbrowse-openclaw-v0.1.22.zip`. Place the extracted `agentbrowse/` folder directly inside `skills/` or `~/.openclaw/skills/`.
Treat AgentBrowse as the browser layer for launch, attach, page observation, actions, navigation, extract, and screenshots.
If I ask for AI-assisted `observe` (with a natural-language goal) or any `extract`, ask me for my API key and run `agentbrowse init <your-api-key>`.
If `agentbrowse` is missing, install or repair `@mercuryo-ai/agentbrowse-cli`.
Verify the setup by using AgentBrowse on a small browser task. If AI-assisted `observe` or `extract` still fails after init, run `agentbrowse doctor`.
```

## What your agent should do

1. Download `agentbrowse-openclaw-v0.1.22.zip` from https://github.com/MercuryoAI/skills/releases/tag/agentbrowse-v0.1.22.
2. Extract the archive so the top-level folder is `agentbrowse/`.
3. Place `agentbrowse/` directly inside `skills/` or `~/.openclaw/skills/`.
4. If `agentbrowse` is missing, install or repair `@mercuryo-ai/agentbrowse-cli`.
5. Only if the task needs AI-assisted `observe` or any `extract`, request your API key and run `agentbrowse init <your-api-key>`.
6. Start a fresh OpenClaw session if the current session does not pick up the installed skill.

Supported skill locations:

- `skills/`
- `~/.openclaw/skills/`

## Verify the result

1. Ask OpenClaw to use AgentBrowse for a small browser task such as launch, attach, or observe.
2. If you need AI-assisted `observe` (natural-language goal) or any `extract`, ask OpenClaw to run `agentbrowse init <your-api-key>` and then retry.
3. If AI-assisted `observe` or `extract` still fails after init, use `agentbrowse doctor` to inspect the local config.

## Try a first task

Start with a real browser task: open or attach to a page, inspect the visible state, and act from there. Add `agentbrowse init <your-api-key>` only when you want AI-assisted `observe` (natural-language goal) or structured `extract`.

- Use AgentBrowse to launch the browser, open this page, and summarize the visible pricing table.
- Use AgentBrowse to attach to the already running browser and continue the search flow from the current page.
- Use AgentBrowse to extract the visible shipment options after running `agentbrowse init <your-api-key>`.

## Manual zip fallback

Use this fallback only if you want to manage the `agentbrowse/` folder yourself instead of asking OpenClaw to install it from https://github.com/MercuryoAI/skills/releases/tag/agentbrowse-v0.1.22 using `agentbrowse-openclaw-v0.1.22.zip`.

1. Download `agentbrowse-openclaw-v0.1.22.zip`.
2. Extract the archive. You should get a folder named `agentbrowse/`.
3. Move that folder into one of the supported install locations below.
4. Restart OpenClaw if you already had a session open.

## Update or reinstall

1. Ask your agent to download the newer archive for this OpenClaw runtime and replace the existing skill folder with the new `agentbrowse/` folder.
2. Keep the folder name exactly `agentbrowse` inside the same skills directory.
3. If you use AI-assisted `observe` or `extract` and the local config was reset, rerun `agentbrowse init <your-api-key>`.
4. Start a fresh OpenClaw session after the replacement.

## Troubleshooting

- **Skill not recognized by OpenClaw**: Make sure the folder sits directly inside `skills/` or `~/.openclaw/skills/`.
- **`agentbrowse` command not found**: Ask the agent to install or repair `@mercuryo-ai/agentbrowse-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/agentbrowse-cli@latest`, then verify with `agentbrowse --version`.
- **AI-assisted `observe` or `extract` needs API access**: Ask the agent to run `agentbrowse init <your-api-key>`, or set `MAGICPAY_API_KEY` in the environment before retrying.
- **AI-assisted `observe` or `extract` still fails after init**: Ask the agent to run `agentbrowse doctor` and inspect the local config.
- **Browser launch or attach is unavailable**: Provide a live CDP endpoint or use an environment that allows browser startup.
- **The task reaches a protected login or payment step**: Keep AgentBrowse for browser control and switch the protected step to MagicPay or MagicPay Agent.

---

This guide is for AgentBrowse v0.1.22.
