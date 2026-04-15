# Install MagicPay in Claude Code

MagicPay gives Claude Code a protected-step workflow for prepared browser pages: attach to the current browser, bind a workflow session, discover the supported login, identity, or payment form, and fill it with values the user has approved through MagicPay — using `magicpay find-form` and `magicpay resolve-form <fillRef>` as the main form flow.

## Before you start

- Node.js 18 or later is installed.
- Claude Code is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The relevant login, identity, or payment page is already open, or another tool can provide a CDP endpoint for it.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill in Claude Code from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.9 using `magicpay-claude-code-v0.1.9.zip`. Place the extracted `magicpay/` folder directly inside `.claude/skills/` or `~/.claude/skills/`.
Treat MagicPay as the protected-form layer for a prepared login, identity, or payment page.
Ask me for my API key and run `magicpay init <your-api-key>`.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
The main form flow is `magicpay find-form` -> `magicpay resolve-form <fillRef>`. Use `magicpay run-action <capability> --params-json <json>` for protected actions that are not form-fills. `magicpay submit-form <fillRef>` is a manual recovery step — only use it when `resolve-form` explicitly left the form unsubmitted.
```

## What your agent should do

1. Download `magicpay-claude-code-v0.1.9.zip` from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.9.
2. Extract the archive so the top-level folder is `magicpay/`.
3. Place `magicpay/` directly inside `.claude/skills/` or `~/.claude/skills/`.
4. If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
5. Request your API key and run `magicpay init <your-api-key>`.
6. Start a fresh Claude Code session if the current session does not pick up the installed skill.

Supported skill locations:

- `.claude/skills/`
- `~/.claude/skills/`

## Verify the result

1. Ask Claude Code to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask Claude Code to attach to a prepared protected form, run `magicpay find-form`, and complete the main form flow with `magicpay resolve-form <fillRef>`.

## Try a first task

Use MagicPay when the browser is already on the relevant login, identity, or payment form. The main form flow is `magicpay find-form` -> `magicpay resolve-form <fillRef>`. Use `magicpay run-action <capability> --params-json <json>` for protected actions that are not form-fills.

- Use MagicPay to attach to the already open checkout page, run `magicpay find-form`, and finish with `magicpay resolve-form <fillRef>` for the checkout form.
- Use MagicPay to continue the prepared login page; let MagicPay choose the request path and stop if `magicpay find-form` cannot confidently identify the form.
- Use MagicPay on the already prepared identity-verification form and ask me before picking between multiple supported forms.

## Manual zip fallback

Use this fallback only if you want to manage the `magicpay/` folder yourself instead of asking Claude Code to install it from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.9 using `magicpay-claude-code-v0.1.9.zip`.

1. Download `magicpay-claude-code-v0.1.9.zip`.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into one of the supported install locations below.
4. Restart Claude Code if you already had a session open.

## Update or reinstall

1. Ask your agent to download the newer archive for this Claude Code runtime and replace the existing skill folder with the new `magicpay/` folder.
2. Keep the folder name exactly `magicpay` inside the same skills directory.
3. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
4. Start a fresh Claude Code session after the replacement.

## Troubleshooting

- **Skill not recognized by Claude Code**: Make sure the folder sits directly inside `.claude/skills/` or `~/.claude/skills/`.
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicpay-cli@latest`, then verify with `magicpay --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config.
- **`magicpay find-form` cannot confidently identify the form**: Confirm the browser is still on the intended page, then rerun `magicpay find-form`.
- **Need to force a submit after resolution**: Use `magicpay submit-form <fillRef>` only if `magicpay resolve-form <fillRef>` reported that automatic submit was skipped, or if you intentionally paused before submit on a fresh form snapshot.
- **No prepared browser context**: Open the target page in a browser first and provide a live CDP endpoint before invoking magicpay.

---

This guide is for MagicPay v0.1.9.
