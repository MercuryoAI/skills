# Install MagicPay Agent in Codex

MagicPay Agent lets Codex run end-to-end browser workflows with payment, login, approval, and protected secret handling through the combined magicpay-agent CLI.

## What you need before installing

- Node.js 18 or later is installed.
- Codex is installed and working.
- You have a MagicPay account and API key. Sign up at https://magicpay.mercuryo.io if needed.

## CLI Surface

- CLI package: `@mercuryo-ai/magicpay-agent-cli`
- CLI binary: `magicpay-agent`
- Install command: `npm i -g @mercuryo-ai/magicpay-agent-cli`

## Install

- Release archive: `magicpay-agent-codex-v0.1.0.zip`
- Release page: https://github.com/MercuryoAI/skills/releases/tag/magicpay-agent-v0.1.0

1. Download the release archive listed above.
2. Extract the archive. You should get a folder named `magicpay-agent/`.
3. Move that folder into one of the supported install locations below.
4. Restart Codex if you already had a session open.

Supported install locations:

- `$CODEX_HOME/skills/`
- `~/.codex/skills/`

## Verify the install

1. Open a new Codex session in your repository.
2. Ask Codex to use MagicPay Agent for a simple browser task.
3. Confirm the agent starts by checking `magicpay-agent --version` and `magicpay-agent status`.
4. If Codex does not use MagicPay Agent, make sure `magicpay-agent/` sits directly inside a supported install location.

## Try it out

Once the skill is installed, ask Codex to use MagicPay Agent for a real website task.

- Use MagicPay Agent to buy the mass gainer on this page, run `magicpay-agent find-form` on the protected checkout form, ask for my approval before entering payment details, and finish with `magicpay-agent submit-form <fillRef>`.
- Use MagicPay Agent to sign in to my account using stored credentials.
- Continue the checkout. The browser is already open on the shipping page.

## Update or reinstall

1. Download the newer release archive for your version.
2. Remove the old `magicpay-agent/` folder from your install location.
3. Extract the new archive and place the new `magicpay-agent/` folder in the same location.
4. Start a new Codex session.

## Troubleshooting

- **Skill not recognized by Codex**: Make sure the folder is named `magicpay-agent` and sits directly inside `$CODEX_HOME/skills/` or `~/.codex/skills/`.
- **`magicpay-agent` command not found**: The agent will try to install it automatically. If that fails, run `npm i -g @mercuryo-ai/magicpay-agent-cli` and try again.
- **Missing API key**: Run `magicpay-agent init <your-api-key>` or provide the key to Codex and ask it to run init for you. Get a key at https://magicpay.mercuryo.io.
- **`magicpay-agent status` still fails after init**: Run `magicpay-agent doctor` to inspect the local `~/.magicpay/config.json` file. Use `doctor` only for local config diagnostics, not as a required first step.
- **Browser does not open**: Make sure the environment allows browser startup. MagicPay Agent needs a real or headless browser.

---

This guide is for MagicPay Agent v0.1.0.
