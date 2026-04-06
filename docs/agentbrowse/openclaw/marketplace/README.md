# Install AgentBrowse from ClawHub

AgentBrowse lets OpenClaw launch or attach to a browser, observe the visible page state, act on interactive targets, use MagicPay-backed goal observe and extract, and close the browser through the browser-only agentbrowse CLI.

## What you need before installing

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- Goal-driven `observe` and `extract` require a MagicPay account and API key. Sign up at https://magicpay.mercuryo.io if needed.
- The environment allows browser launch or provides a CDP endpoint for attach.

## Install

- Preferred path: ask your agent to install the `agentbrowse` skill from ClawHub in the current workspace.
- If the task needs goal-driven `observe` or `extract`, ask the agent to request your MagicPay API key and run `agentbrowse init <your-api-key>`.
- ClawHub slug: `agentbrowse`
- Release page: https://github.com/MercuryoAI/skills/releases/tag/agentbrowse-v0.1.1

1. Ask your OpenClaw agent to install the `agentbrowse` skill from ClawHub in this workspace.
2. If you plan to use goal-driven `observe` or any `extract`, ask the agent to request your MagicPay API key and run `agentbrowse init <your-api-key>`.
3. Ask the agent to confirm the install by running `agentbrowse --version`.
4. If the current OpenClaw session still does not see the skill after install or init, start a fresh OpenClaw session.

## Verify the install

1. Ask OpenClaw to confirm the skill is installed and run `agentbrowse --version`.
2. Ask OpenClaw to use AgentBrowse for a simple browser-only task.
3. If the task needs goal-driven `observe` or `extract`, confirm the agent already completed `agentbrowse init <your-api-key>`.

## Try it out

Let the agent handle the ClawHub install first, then use AgentBrowse for a simple browser-only task. Only let the agent request your MagicPay API key and run init when you actually need semantic observe or extract.

- Use AgentBrowse to launch the browser, open this page, and summarize the visible pricing table.
- Use AgentBrowse to attach to the already running browser and click through the search form.
- Use AgentBrowse to extract the visible shipment options from the current page after running `agentbrowse init <your-api-key>`.

## Manual zip fallback

Use the manual path only if you want to manage the skill files yourself instead of letting OpenClaw install from ClawHub.

1. Download the public release archive for the current AgentBrowse skill version.
2. Extract the archive. You should get a folder named `agentbrowse/`.
3. Move that folder into `skills/` or `~/.openclaw/skills/`.
4. Restart OpenClaw if you already had a session open.

## Update or reinstall

1. Ask your agent to update the `agentbrowse` skill from ClawHub.
2. If the current session still does not see the updated skill, start a fresh OpenClaw session.

## Troubleshooting

- **Skill not available after install**: Ask the agent to verify that `agentbrowse` is installed, then start a fresh OpenClaw session if the current one still does not see it.
- **`agentbrowse` command not found**: Ask the agent to install or repair `@mercuryo-ai/agentbrowse-cli`. If you want to do it yourself, run `npm i -g @mercuryo-ai/agentbrowse-cli`.
- **Goal observe or extract needs backend access**: Ask the agent to request your API key and run `agentbrowse init <your-api-key>` before retrying.
- **Need a manual fallback instead of ClawHub**: Use the direct-install guide from the public skills repo release and place `agentbrowse/` under an OpenClaw skills directory yourself.

---

This guide is for AgentBrowse v0.1.1.
