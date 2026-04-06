# Install MagicPay in Claude Code

MagicPay lets Claude Code attach to an already prepared browser page, bind a workflow session, discover a supported protected form, request approval for stored secrets, fill approved values, and submit the form safely.

## What you need before installing

- Node.js 18 or later is installed.
- Claude Code is installed and working.
- You have a MagicPay account and API key. Sign up at https://magicpay.mercuryo.io if needed.
- Another tool or agent can prepare the browser and provide a CDP endpoint for the target page.

## CLI Surface

- CLI package: `@mercuryo-ai/magicpay-cli`
- CLI binary: `magicpay`
- Install command: `npm i -g @mercuryo-ai/magicpay-cli`

## Install

- Release archive: `magicpay-claude-code-v0.1.0.zip`
- Release page: https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.0

1. Download the release archive listed above.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into one of the supported install locations below.
4. Restart Claude Code if you already had a session open.

Supported install locations:

- `.claude/skills/`
- `~/.claude/skills/`

## Verify the install

1. Open a new Claude Code session in any project.
2. Ask Claude Code to use MagicPay on a prepared login, identity, or payment form.
3. Confirm the agent starts by checking `magicpay --version` and `magicpay status`.
4. If Claude Code does not use MagicPay, make sure `magicpay/` sits directly inside a supported install location.

## Try it out

Once the skill is installed, ask Claude Code to use MagicPay for a prepared protected-form task.

- Use MagicPay to attach to the already open checkout page, run `magicpay find-form`, request approval for my stored card, fill it, and finish with `magicpay submit-form <fillRef>`.
- Use MagicPay to continue the prepared login page with stored credentials.
- Use MagicPay on the already prepared identity-verification form and stop if the form is ambiguous.

## Update or reinstall

1. Download the newer release archive for your version.
2. Remove the old `magicpay/` folder from your install location.
3. Extract the new archive and place the new `magicpay/` folder in the same location.
4. Start a new Claude Code session.

## Troubleshooting

- **Skill not recognized by Claude Code**: Make sure the folder is named `magicpay` and sits directly inside `.claude/skills/` or `~/.claude/skills/`.
- **`magicpay` command not found**: The agent will try to install it automatically. If that fails, run `npm i -g @mercuryo-ai/magicpay-cli` and try again.
- **Missing API key**: Run `magicpay init <your-api-key>` or provide the key to Claude Code and ask it to run init for you. Get a key at https://magicpay.mercuryo.io.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local `~/.magicpay/config.json` file. Use `doctor` only for local config diagnostics, not as a required first step.
- **No browser context**: Provide a live CDP endpoint for the prepared browser page before invoking MagicPay.

---

This guide is for MagicPay v0.1.0.
