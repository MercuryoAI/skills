# Install MagicPay in OpenClaw

MagicPay lets OpenClaw attach to an already prepared browser page, bind a workflow session, discover a supported protected form, request approval for stored secrets, and finish with `magicpay fill-secret <fillRef> <requestId>` on the normal path.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- Another tool or agent can prepare the browser and provide a CDP endpoint for the target page.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill in OpenClaw from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.2 using `magicpay-openclaw-v0.1.2.zip`. Place the extracted `magicpay/` folder directly inside `skills/` or `~/.openclaw/skills/`.
Ask me for my MagicPay API key and run `magicpay init <your-api-key>`.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay --version` and `magicpay status`; use `magicpay doctor` only if status still fails after init.
For the normal protected-form flow, finish with `magicpay fill-secret <fillRef> <requestId>`; use `magicpay submit-form <fillRef>` only for manual recovery.
```

## What your agent should do

1. Download `magicpay-openclaw-v0.1.2.zip` from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.2.
2. Extract the archive so the top-level folder is `magicpay/`.
3. Place `magicpay/` directly inside `skills/` or `~/.openclaw/skills/`.
4. If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
5. Request your MagicPay API key and run `magicpay init <your-api-key>`.
6. Start a fresh OpenClaw session if the current session does not pick up the installed skill.

Supported skill locations:

- `skills/`
- `~/.openclaw/skills/`

## Verify the result

1. Ask OpenClaw to run `magicpay --version` and `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask OpenClaw to attach to a prepared protected form, run `magicpay find-form`, request approval for a stored secret, and finish with `magicpay fill-secret <fillRef> <requestId>`.

## Try a first task

Use MagicPay when the page and protected form are already open. The normal path ends with `magicpay fill-secret <fillRef> <requestId>`; `magicpay submit-form <fillRef>` is only for manual recovery.

- Use MagicPay to attach to the already open checkout page, run `magicpay find-form`, request approval for my stored card, and finish with `magicpay fill-secret <fillRef> <requestId>`.
- Use MagicPay to continue the prepared login page with stored credentials and finish with `magicpay fill-secret <fillRef> <requestId>`.
- Use MagicPay on the already prepared identity-verification form and stop if the form is ambiguous.

## Manual zip fallback

Use this fallback only if you want to manage the `magicpay/` folder yourself instead of asking OpenClaw to install it from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.2 using `magicpay-openclaw-v0.1.2.zip`.

1. Download `magicpay-openclaw-v0.1.2.zip`.
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
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicpay-cli`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config. Use `doctor` for diagnostics only, not as a required first step.
- **Need to force a submit after fill**: Use `magicpay submit-form <fillRef>` only if `magicpay fill-secret <fillRef> <requestId>` reported that automatic submit was skipped or if you intentionally paused before submit on a fresh form snapshot.
- **No prepared browser context**: Open the target page in a browser first and provide a live CDP endpoint before invoking magicpay.

---

This guide is for MagicPay v0.1.2.
