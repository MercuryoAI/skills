# Install MagicBrowse in Codex

MagicBrowse gives Codex a browser automation fallback for real web pages: launch a fresh Chrome session, or attach to an explicitly approved CDP session, drive it with natural-language steps through `magicbrowse act`, and use deterministic `magicbrowse observe` plus `click`/`type`/`fill`/`select`/`press` primitives when single-element precision is required. It stops at protected forms (login, identity, payment), hands those steps to MagicPay, and asks before consequential actions such as submit, post, book, buy, save, or delete.

## Before you start

- Node.js 18 or later is installed.
- Codex is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The environment can launch a Chrome session. Use `magicbrowse attach` only for a private CDP endpoint explicitly approved for the current task.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicbrowse` skill in Codex from https://github.com/MercuryoAI/skills/releases/tag/magicbrowse-v0.1.3 using `magicbrowse-codex-v0.1.3.zip`. Place the extracted `magicbrowse/` folder directly inside `$CODEX_HOME/skills/` or `~/.codex/skills/`.
Treat MagicBrowse as a browser automation fallback. Use it only when your own browser tooling cannot reach a target page reliably.
Ask me for my API key and run `magicbrowse init <your-api-key>`, or set `MAGICPAY_API_KEY` in the environment. The persisted config lives at `~/.magicpay/config.json`, shared with MagicPay.
If `magicbrowse` is missing, install or repair `@mercuryo-ai/magicbrowse-cli`.
Verify the setup with `magicbrowse doctor`. The primary workflow is `magicbrowse launch <url>` -> one or more `magicbrowse act "<goal>"` -> `magicbrowse close`.
Use a fresh browser by default. Do not attach to an existing CDP endpoint, named profile, or user-data directory unless I explicitly approve that browser/session for this task.
Stop at protected forms (login, identity, payment) and switch to MagicPay. Also ask me before submitting, posting, sending, saving, deleting, booking, buying, ordering, paying, accepting terms, or changing account data/settings.
```

## What your agent should do

1. Download `magicbrowse-codex-v0.1.3.zip` from https://github.com/MercuryoAI/skills/releases/tag/magicbrowse-v0.1.3.
2. Extract the archive so the top-level folder is `magicbrowse/`.
3. Place `magicbrowse/` directly inside `$CODEX_HOME/skills/` or `~/.codex/skills/`.
4. If `magicbrowse` is missing, install or repair `@mercuryo-ai/magicbrowse-cli`.
5. Request your API key and run `magicbrowse init <your-api-key>`.
6. Start a fresh Codex session if the current session does not pick up the installed skill.

Supported skill locations:

- `$CODEX_HOME/skills/`
- `~/.codex/skills/`

## Verify the result

1. Ask Codex to run `magicbrowse doctor`.
2. If `magicbrowse doctor` still fails after init, inspect `~/.magicpay/config.json` for a malformed key or wrong gateway URL.
3. Ask Codex to use MagicBrowse on a small `magicbrowse launch <url>` -> `magicbrowse act "<goal>"` -> `magicbrowse close` workflow.

## Try a first task

Use MagicBrowse as a fallback when your own browser tooling cannot reach a target page reliably. The primary workflow is `magicbrowse launch <url>` -> one or more `magicbrowse act "<goal>"` -> `magicbrowse close`. Run `magicbrowse doctor` once after install to verify the shared MagicPay gateway config.

- Use MagicBrowse to `magicbrowse launch <url>` and `magicbrowse act "summarize the visible pricing table"`.
- Use MagicBrowse to `magicbrowse launch <meta-search-url>` and `magicbrowse act "search one-way flights London → Lisbon next Tuesday and reach the passenger details page"`.
- Use MagicBrowse to `magicbrowse launch <support-url>` and `magicbrowse act "find the return-policy section and summarize the steps without submitting anything"`.

## Manual zip fallback

Use this fallback only if you want to manage the `magicbrowse/` folder yourself instead of asking Codex to install it from https://github.com/MercuryoAI/skills/releases/tag/magicbrowse-v0.1.3 using `magicbrowse-codex-v0.1.3.zip`.

1. Download `magicbrowse-codex-v0.1.3.zip`.
2. Extract the archive. You should get a folder named `magicbrowse/`.
3. Move that folder into one of the supported install locations below.
4. Restart Codex if you already had a session open.

## Update or reinstall

1. Ask your agent to download the newer archive for this Codex runtime and replace the existing skill folder with the new `magicbrowse/` folder.
2. Keep the folder name exactly `magicbrowse` inside the same skills directory.
3. If prompted, provide your API key again and rerun `magicbrowse init <your-api-key>`.
4. Start a fresh Codex session after the replacement.

## Troubleshooting

- **Skill not recognized by Codex**: Make sure the folder sits directly inside `$CODEX_HOME/skills/` or `~/.codex/skills/`.
- **`magicbrowse` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicbrowse-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicbrowse-cli@0.0.5`, then verify with `magicbrowse --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicbrowse init <your-api-key>`. The persisted config lives at `~/.magicpay/config.json`, shared with MagicPay.
- **`magicbrowse doctor` still fails after init**: Inspect `~/.magicpay/config.json` for a malformed key or wrong gateway URL.
- **Browser launch is unavailable**: Use an environment that allows Chrome startup, or explicitly approve a private CDP endpoint for this task before using attach.
- **`magicbrowse act` returns `status: completed` but with a `finalMessage` asking for login or captcha**: That is expected. Surface the message; do not retry or script around it.
- **`magicbrowse act` returns `status: max_steps`**: The granule was too large or vague. Split the task on a page-change boundary or tighten the goal's expected terminal state.
- **The task reaches a protected login, identity, or payment form**: Stop using MagicBrowse on the form and switch to MagicPay for the protected step.
- **The next step would submit, post, send, save, delete, book, buy, order, pay, or accept terms**: Stop and ask for explicit approval before executing that final action.
- **Concurrent workflows clobbering each other**: Set a distinct `MAGICBROWSE_HOME` per workflow; the default `~/.magicbrowse/current-session.json` is a singleton.

---

This guide is for MagicBrowse v0.1.3.
