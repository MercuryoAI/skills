# Install MagicPay in Claude Code

MagicPay gives Claude Code a protected-form workflow for prepared browser pages: attach to the current browser, bind a workflow session, discover the supported login, identity, or payment form, request approval for stored values, and finish with `magicpay fill-secret <fillRef> <requestId>` on the normal path.

## Before you start

- Node.js 18 or later is installed.
- Claude Code is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The relevant login, identity, or payment page is already open, or another tool can provide a CDP endpoint for it.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill in Claude Code from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.2 using `magicpay-claude-code-v0.1.2.zip`. Place the extracted `magicpay/` folder directly inside `.claude/skills/` or `~/.claude/skills/`.
Treat MagicPay as the protected-form layer for a prepared login, identity, or payment page.
Ask me for my API key and run `magicpay init <your-api-key>`.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay status`; use `magicpay doctor` only if status still fails after init.
For the normal protected-form flow, finish with `magicpay fill-secret <fillRef> <requestId>`. Use `magicpay submit-form <fillRef>` only for manual recovery.
```

## What your agent should do

1. Download `magicpay-claude-code-v0.1.2.zip` from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.2.
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
3. Ask Claude Code to attach to a prepared protected form, run `magicpay find-form`, request approval for a stored secret, and finish with `magicpay fill-secret <fillRef> <requestId>`.

## Try a first task

Use MagicPay when the browser is already on the relevant login, identity, or payment form. The normal path is `magicpay find-form` -> `magicpay request-secret` -> `magicpay poll-secret` -> `magicpay fill-secret <fillRef> <requestId>`.

- Use MagicPay to attach to the already open checkout page, run `magicpay find-form`, request approval for my stored card, and finish with `magicpay fill-secret <fillRef> <requestId>`.
- Use MagicPay to continue the prepared login page with stored credentials and stop if the form mapping becomes ambiguous.
- Use MagicPay on the already prepared identity-verification form and ask me before choosing between multiple supported forms.

## Manual zip fallback

Use this fallback only if you want to manage the `magicpay/` folder yourself instead of asking Claude Code to install it from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.2 using `magicpay-claude-code-v0.1.2.zip`.

1. Download `magicpay-claude-code-v0.1.2.zip`.
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
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicpay-cli`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config. Use `doctor` for diagnostics only, not as a required first step.
- **Protected form is not found or is ambiguous**: Confirm the browser is still on the intended page, then rerun `magicpay find-form`.
- **Need to force a submit after fill**: Use `magicpay submit-form <fillRef>` only if `magicpay fill-secret <fillRef> <requestId>` reported that automatic submit was skipped or if you intentionally paused before submit on a fresh form snapshot.
- **No prepared browser context**: Open the target page in a browser first and provide a live CDP endpoint before invoking magicpay.

---

This guide is for MagicPay v0.1.2.
