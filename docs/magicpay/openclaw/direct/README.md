# Install MagicPay in OpenClaw

MagicPay helps OpenClaw handle approved login, identity, checkout, donation, subscription, and payment pages: attach to the approved browser session, bind a workflow session, discover the supported form, and fill it for review with values the user has approved through MagicPay. The main path is `magicpay find-form` -> `magicpay resolve-form <fillRef> --no-submit` -> explicit user approval -> `magicpay submit-form <fillRef>`; sensitive actions use `magicpay run-action` only after the user approves the capability and params. MagicPay completes its workflow with `magicpay end-session`, but browser cleanup remains owned by the browser tool or orchestrator that prepared the page.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The relevant login, identity, checkout, donation, subscription, or payment page is already open in an approved browser session, or another tool can provide an approved private CDP endpoint for it.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill in OpenClaw from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.21 using `magicpay-openclaw-v0.1.21.zip`. Place the extracted `magicpay/` folder directly inside `skills/` or `~/.openclaw/skills/`.
Treat MagicPay as the helper for prepared login, identity, checkout, donation, subscription, and payment pages.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
Use only the prepared browser/session I approve for this task. Keep my API key, local config, CDP endpoint, and vault item ids private.
The main form flow is `magicpay find-form` -> `magicpay resolve-form <fillRef> --no-submit` -> ask me to approve the exact site/merchant, action, and visible amount or data -> `magicpay submit-form <fillRef>` only if I approve. Use `magicpay run-action <capability> --params-json <json>` only after I approve the capability and params.
`magicpay end-session` completes only the MagicPay workflow. Browser lifecycle remains owned by the browser tool or orchestrator that prepared the page: clean up an owned disposable browser only after the overall task is done and never close an external or user-owned browser without my explicit teardown approval.
Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is confirmed present on the current MagicPay-attached browser session.
```

## What your agent should do

1. Download `magicpay-openclaw-v0.1.21.zip` from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.21.
2. Extract the archive so the top-level folder is `magicpay/`.
3. Place `magicpay/` directly inside `skills/` or `~/.openclaw/skills/`.
4. If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
5. Request your API key and run `magicpay init <your-api-key>`; omit `--api-url` unless you need a non-default gateway.
6. Start a fresh OpenClaw session if the current session does not pick up the installed skill.

Supported skill locations:

- `skills/`
- `~/.openclaw/skills/`

## Verify the result

1. Ask OpenClaw to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask OpenClaw to attach to an approved prepared login, identity, checkout, donation, subscription, or payment page, run `magicpay find-form`, and fill for review with `magicpay resolve-form <fillRef> --no-submit`.

## Try a first task

Use MagicPay when the browser is already on the relevant login, identity, checkout, donation, subscription, or payment page and the user approved that browser/session for this task. The main form path is `magicpay find-form` -> `magicpay resolve-form <fillRef> --no-submit` -> explicit user approval -> `magicpay submit-form <fillRef>`. Use `magicpay run-action <capability> --params-json <json>` only after the user approves the capability and params. `magicpay end-session` ends the MagicPay workflow, then the browser owner decides whether to keep or clean up the browser.

- Use MagicPay to attach to the approved checkout page, run `magicpay find-form`, and fill for review with `magicpay resolve-form <fillRef> --no-submit`; ask before submit.
- Use MagicPay to continue the prepared login page; use `magicpay resolve-form <fillRef> --no-submit` and ask before final login submit.
- Use MagicPay on the prepared donation or subscription checkout page; fill for review and ask before confirming the amount or plan.
- Use MagicPay on the already prepared identity-verification form and ask me before picking between multiple supported forms or submitting identity data.

## Manual zip fallback

Use this fallback only if you want to manage the `magicpay/` folder yourself instead of asking OpenClaw to install it from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.21 using `magicpay-openclaw-v0.1.21.zip`.

1. Download `magicpay-openclaw-v0.1.21.zip`.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into one of the supported install locations below.
4. Restart OpenClaw if you already had a session open.

## Update or reinstall

1. Ask your agent to download the newer archive for this OpenClaw runtime and replace the existing skill folder with the new `magicpay/` folder.
2. Keep the folder name exactly `magicpay` inside the same skills directory.
3. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
4. Start a fresh OpenClaw session after the replacement.

## Troubleshooting

- **Skill not recognized by OpenClaw**: Make sure the folder sits directly inside `skills/` or `~/.openclaw/skills/`.
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicpay-cli@latest`, then verify with `magicpay --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config.
- **`magicpay find-form` cannot confidently identify the form**: Confirm the browser is still on the intended page, then rerun `magicpay find-form`.
- **Confirmed CAPTCHA on the current page**: Use `magicpay solve-captcha [--timeout <s>]` on the current MagicPay-attached browser session, then continue the normal browser or MagicPay form flow.
- **Need to submit after resolution**: Use `magicpay submit-form <fillRef>` only after the user approves the current site/merchant, action, and visible amount or data on a fresh form snapshot.
- **No prepared browser context**: Open the target page in an approved browser session first and provide a private CDP endpoint before invoking magicpay.

---

This guide is for MagicPay v0.1.21.
