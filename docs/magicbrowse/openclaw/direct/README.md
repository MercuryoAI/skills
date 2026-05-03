# Install MagicBrowse in OpenClaw

MagicBrowse gives OpenClaw an LLM-first browser autonomy fallback for real web pages: launch or attach to a Chrome session, drive it with natural-language goals through `magicbrowse act`, and use deterministic `magicbrowse observe` plus `click`/`type`/`fill`/`select`/`press` primitives when autonomy needs single-element precision. Stops at the boundary of any protected form (login, identity, payment) and hands off to MagicPay.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The environment can launch a Chrome session, or another tool can provide a CDP endpoint for `magicbrowse attach`.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicbrowse` skill in OpenClaw from https://github.com/MercuryoAI/skills/releases/tag/magicbrowse-v0.1.0 using `magicbrowse-openclaw-v0.1.0.zip`. Place the extracted `magicbrowse/` folder directly inside `skills/` or `~/.openclaw/skills/`.
Treat MagicBrowse as the LLM-first browser autonomy fallback. Use it only when your own browser tooling cannot reach a target page reliably.
Ask me for my API key and run `magicbrowse init <your-api-key>`, or set `MAGICPAY_API_KEY` in the environment. The persisted config lives at `~/.magicpay/config.json`, shared with MagicPay.
If `magicbrowse` is missing, install or repair `@mercuryo-ai/magicbrowse-cli`.
Verify the setup with `magicbrowse doctor`. The primary workflow is `magicbrowse launch <url>` -> one or more `magicbrowse act "<goal>"` -> `magicbrowse close`.
Stop at the boundary of any protected form (login, identity, payment) and switch to MagicPay.
```

## What your agent should do

1. Download `magicbrowse-openclaw-v0.1.0.zip` from https://github.com/MercuryoAI/skills/releases/tag/magicbrowse-v0.1.0.
2. Extract the archive so the top-level folder is `magicbrowse/`.
3. Place `magicbrowse/` directly inside `skills/` or `~/.openclaw/skills/`.
4. If `magicbrowse` is missing, install or repair `@mercuryo-ai/magicbrowse-cli`.
5. Request your API key and run `magicbrowse init <your-api-key>`.
6. Start a fresh OpenClaw session if the current session does not pick up the installed skill.

Supported skill locations:

- `skills/`
- `~/.openclaw/skills/`

## Verify the result

1. Ask OpenClaw to run `magicbrowse doctor`.
2. If `magicbrowse doctor` still fails after init, inspect `~/.magicpay/config.json` for a malformed key or wrong gateway URL.
3. Ask OpenClaw to use MagicBrowse on a small `magicbrowse launch <url>` -> `magicbrowse act "<goal>"` -> `magicbrowse close` workflow.

## Try a first task

Use MagicBrowse as a fallback when your own browser tooling cannot reach a target page reliably. The primary workflow is `magicbrowse launch <url>` -> one or more `magicbrowse act "<goal>"` -> `magicbrowse close`. Run `magicbrowse doctor` once after install to verify the shared MagicPay gateway config.

- Use MagicBrowse to `magicbrowse launch <url>` and `magicbrowse act "summarize the visible pricing table"`.
- Use MagicBrowse to `magicbrowse launch <meta-search-url>` and `magicbrowse act "search one-way flights London → Lisbon next Tuesday and reach the passenger details page"`.
- Use MagicBrowse to `magicbrowse attach <cdp-url>` to my running browser and `magicbrowse act "navigate to the order confirmation page"`.

## Manual zip fallback

Use this fallback only if you want to manage the `magicbrowse/` folder yourself instead of asking OpenClaw to install it from https://github.com/MercuryoAI/skills/releases/tag/magicbrowse-v0.1.0 using `magicbrowse-openclaw-v0.1.0.zip`.

1. Download `magicbrowse-openclaw-v0.1.0.zip`.
2. Extract the archive. You should get a folder named `magicbrowse/`.
3. Move that folder into one of the supported install locations below.
4. Restart OpenClaw if you already had a session open.

## Update or reinstall

1. Ask your agent to download the newer archive for this OpenClaw runtime and replace the existing skill folder with the new `magicbrowse/` folder.
2. Keep the folder name exactly `magicbrowse` inside the same skills directory.
3. If prompted, provide your API key again and rerun `magicbrowse init <your-api-key>`.
4. Start a fresh OpenClaw session after the replacement.

## Troubleshooting

- **Skill not recognized by OpenClaw**: Make sure the folder sits directly inside `skills/` or `~/.openclaw/skills/`.
- **`magicbrowse` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicbrowse-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicbrowse-cli@latest`, then verify with `magicbrowse --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicbrowse init <your-api-key>`. The persisted config lives at `~/.magicpay/config.json`, shared with MagicPay.
- **`magicbrowse doctor` still fails after init**: Inspect `~/.magicpay/config.json` for a malformed key or wrong gateway URL.
- **Browser launch or attach is unavailable**: Provide a live CDP endpoint or use an environment that allows browser startup.
- **`magicbrowse act` returns `status: completed` but with a `finalMessage` asking for login or captcha**: That is expected. Surface the message; do not retry or script around it.
- **`magicbrowse act` returns `status: max_steps`**: The granule was too large or vague. Split the task on a page-change boundary or tighten the goal's expected terminal state.
- **The task reaches a protected login, identity, or payment form**: Stop using MagicBrowse on the form and switch to MagicPay for the protected step.
- **Concurrent workflows clobbering each other**: Set a distinct `MAGICBROWSE_HOME` per workflow; the default `~/.magicbrowse/current-session.json` is a singleton.

---

This guide is for MagicBrowse v0.1.0.
