# Install AgentBrowse in Codex

AgentBrowse lets Codex launch or attach to a browser, observe the visible page state, act on interactive targets, use MagicPay-backed goal observe and extract, and close the browser through the browser-only agentbrowse CLI.

## What you need before installing

- Node.js 18 or later is installed.
- Codex is installed and working.
- Goal-driven `observe` and `extract` require a MagicPay account and API key. Sign up at https://magicpay.mercuryo.io if needed.
- The environment allows browser launch or provides a CDP endpoint for attach.

## CLI Surface

- CLI package: `@mercuryo-ai/agentbrowse-cli`
- CLI binary: `agentbrowse`
- Install command: `npm i -g @mercuryo-ai/agentbrowse-cli`

## Install

- Release archive: `agentbrowse-codex-v0.1.3.zip`
- Release page: https://github.com/MercuryoAI/skills/releases/tag/agentbrowse-v0.1.3

1. Download the release archive listed above.
2. Extract the archive. You should get a folder named `agentbrowse/`.
3. Move that folder into one of the supported install locations below.
4. Restart Codex if you already had a session open.

Supported install locations:

- `$CODEX_HOME/skills/`
- `~/.codex/skills/`

## Verify the install

1. Open a new Codex session in your repository.
2. Ask Codex to use AgentBrowse for a simple browser-only task.
3. Confirm the agent starts by checking `agentbrowse --version`.
4. If the task needs goal-driven `observe` or `extract`, run `agentbrowse init <your-api-key>` before retrying the task.
5. If Codex does not use AgentBrowse, make sure `agentbrowse/` sits directly inside a supported install location.

## Try it out

Once the skill is installed, ask Codex to use AgentBrowse for a browser-only task.

- Use AgentBrowse to launch the browser, open this page, and summarize the visible pricing table.
- Use AgentBrowse to attach to the already running browser and click through the search form.
- Use AgentBrowse to extract the visible shipment options from the current page after running `agentbrowse init <your-api-key>`.

## Update or reinstall

1. Download the newer release archive for your version.
2. Remove the old `agentbrowse/` folder from your install location.
3. Extract the new archive and place the new `agentbrowse/` folder in the same location.
4. Start a new Codex session.

## Troubleshooting

- **Skill not recognized by Codex**: Make sure the folder is named `agentbrowse` and sits directly inside `$CODEX_HOME/skills/` or `~/.codex/skills/`.
- **`agentbrowse` command not found**: The agent will try to install it automatically. If that fails, run `npm i -g @mercuryo-ai/agentbrowse-cli` and try again.
- **Goal observe or extract needs backend access**: Run `agentbrowse init <your-api-key>` or provide `MAGICPAY_API_KEY` before using semantic observe or extract.
- **Goal observe or extract still fail after init**: Run `agentbrowse doctor` to inspect the local `~/.magicpay/config.json` file. Use `doctor` only when the setup is supposed to come from local config.
- **Browser does not open**: Make sure the environment allows browser startup, or provide a CDP endpoint for attach.
- **Task needs protected secrets**: Hand off to MagicPay or MagicPay Agent instead of entering sensitive values manually.

---

This guide is for AgentBrowse v0.1.3.
