# Install MagicPay in Claude Code

MagicPay helps Claude Code handle approved login, identity, checkout, donation, subscription, and payment pages: attach to the approved browser session, bind a workflow session, discover the supported form, and fill it with values the user has approved through MagicPay. The main path is `magicpay find-form` -> `magicpay resolve-form <fillRef>`; after the protected fill completes, continue the page through the browser owner, using a MagicBrowse protected-form `handoff.resumeObjective` when one was returned. Sensitive actions use `magicpay run-action` only after the user approves the capability and params. MagicPay completes its workflow with `magicpay end-session`, but browser cleanup remains owned by the browser tool or orchestrator that prepared the page.

## Before you start

- Node.js 18 or later is installed.
- Claude Code is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The relevant login, identity, checkout, donation, subscription, or payment page is already open in an approved browser session, or another tool can provide an approved private CDP endpoint for it.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill in Claude Code from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.22 using `magicpay-claude-code-v0.1.22.zip`. Place the extracted `magicpay/` folder directly inside `.claude/skills/` or `~/.claude/skills/`.
Treat MagicPay as the helper for prepared login, identity, checkout, donation, subscription, and payment pages.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
Use only the prepared browser/session I approve for this task. Keep my API key, local config, CDP endpoint, and vault item ids private.
The main form flow is `magicpay find-form` -> `magicpay resolve-form <fillRef>`; after MagicPay fills the protected form, continue through the browser tool or orchestrator that prepared the page. If MagicBrowse returned a protected-form `handoff.resumeObjective`, use that as the next `magicbrowse act` goal. Use `magicpay run-action <capability> --params-json <json>` only after I approve the capability and params.
`magicpay end-session` completes only the MagicPay workflow. Browser lifecycle remains owned by the browser tool or orchestrator that prepared the page: clean up an owned disposable browser only after the overall task is done and never close an external or user-owned browser without my explicit teardown approval.
Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is confirmed present on the current MagicPay-attached browser session.
```

## What your agent should do

1. Download `magicpay-claude-code-v0.1.22.zip` from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.22.
2. Extract the archive so the top-level folder is `magicpay/`.
3. Place `magicpay/` directly inside `.claude/skills/` or `~/.claude/skills/`.
4. If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
5. Request your API key and run `magicpay init <your-api-key>`; omit `--api-url` unless you need a non-default gateway.
6. Start a fresh Claude Code session if the current session does not pick up the installed skill.

Supported skill locations:

- `.claude/skills/`
- `~/.claude/skills/`

## Verify the result

1. Ask Claude Code to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask Claude Code to attach to an approved prepared login, identity, checkout, donation, subscription, or payment page, run `magicpay find-form`, fill it with `magicpay resolve-form <fillRef>`, and continue the page through the browser owner.

## Try a first task

Use MagicPay when the browser is already on the relevant login, identity, checkout, donation, subscription, or payment page and the user approved that browser/session for this task. The main form path is `magicpay find-form` -> `magicpay resolve-form <fillRef>`; after the protected fill completes, continue through the browser owner. If MagicBrowse returned a protected-form `handoff.resumeObjective`, use it as the next `magicbrowse act` goal. Use `magicpay run-action <capability> --params-json <json>` only after the user approves the capability and params. `magicpay end-session` ends the MagicPay workflow, then the browser owner decides whether to keep or clean up the browser.

- Use MagicPay to attach to the approved checkout page, run `magicpay find-form`, fill with `magicpay resolve-form <fillRef>`, then continue the checkout through the browser owner.
- Use MagicPay to continue the prepared login page; use `magicpay resolve-form <fillRef>`, then resume the login flow through the browser owner.
- Use MagicPay on the prepared donation or subscription checkout page; fill the protected form, then continue through the browser owner with explicit approval for any consequential confirmation.
- Use MagicPay on the already prepared identity-verification form and ask me before picking between multiple supported forms or submitting identity data.

## Manual zip fallback

Use this fallback only if you want to manage the `magicpay/` folder yourself instead of asking Claude Code to install it from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.22 using `magicpay-claude-code-v0.1.22.zip`.

1. Download `magicpay-claude-code-v0.1.22.zip`.
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
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config.
- **`magicpay find-form` cannot confidently identify the form**: Confirm the browser is still on the intended page, then rerun `magicpay find-form`.
- **Confirmed CAPTCHA on the current page**: Use `magicpay solve-captcha [--timeout <s>]` on the current MagicPay-attached browser session, then continue the normal browser or MagicPay form flow.
- **Need to continue after protected fill**: Continue through the browser owner. If MagicBrowse returned a protected-form handoff, call `magicbrowse act` with `handoff.resumeObjective`; otherwise use the current visible page state to choose the next narrow browser goal.
- **No prepared browser context**: Open the target page in an approved browser session first and provide a private CDP endpoint before invoking magicpay.

---

This guide is for MagicPay v0.1.22.
