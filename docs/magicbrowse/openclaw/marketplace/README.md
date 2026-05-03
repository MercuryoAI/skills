# Install MagicBrowse from ClawHub

MagicBrowse gives OpenClaw a browser automation fallback for real web pages: launch a fresh Chrome session, or attach to an explicitly approved CDP session, drive it with natural-language steps through `magicbrowse act`, and use deterministic `magicbrowse observe` plus `click`/`type`/`fill`/`select`/`press` primitives when single-element precision is required. It stops at protected forms (login, identity, payment), hands those steps to MagicPay, and asks before consequential actions such as submit, post, book, buy, save, or delete.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The environment can launch a Chrome session. Use `magicbrowse attach` only for a private CDP endpoint explicitly approved for the current task.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicbrowse` skill from ClawHub in this workspace.
Treat MagicBrowse as a browser automation fallback. Use it only when your own browser tooling cannot reach a target page reliably.
Ask me for my API key and run `magicbrowse init <your-api-key>`, or set `MAGICPAY_API_KEY` in the environment. The persisted config lives at `~/.magicpay/config.json`, shared with MagicPay.
If `magicbrowse` is missing, install or repair `@mercuryo-ai/magicbrowse-cli`.
Verify the setup with `magicbrowse doctor`. The primary workflow is `magicbrowse launch <url>` -> one or more `magicbrowse act "<goal>"` -> `magicbrowse close`.
Use a fresh browser by default. Do not attach to an existing CDP endpoint, named profile, or user-data directory unless I explicitly approve that browser/session for this task.
Stop at protected forms (login, identity, payment) and switch to MagicPay. Also ask me before submitting, posting, sending, saving, deleting, booking, buying, ordering, paying, accepting terms, or changing account data/settings.
```

## Install source

- Preferred path: ask your agent to install the `magicbrowse` skill from ClawHub in the current workspace.
- After install, ask the agent to request your API key and run `magicbrowse init <your-api-key>`.
- ClawHub slug: `magicbrowse`

## What your agent should do

1. Install the `magicbrowse` skill from ClawHub in the current workspace.
2. Request your API key and run `magicbrowse init <your-api-key>`.
3. Verify the install with `magicbrowse doctor` and a small `launch` -> `act` -> `close` workflow.
4. Start a fresh OpenClaw session if the current session does not see the installed skill.

## Verify the result

1. Ask OpenClaw to run `magicbrowse doctor`.
2. If `magicbrowse doctor` still fails after init, inspect `~/.magicpay/config.json` for a malformed key or wrong gateway URL.
3. Ask OpenClaw to use MagicBrowse on a small `magicbrowse launch <url>` -> `magicbrowse act "<goal>"` -> `magicbrowse close` workflow.

## Try a first task

Use MagicBrowse as a fallback when your own browser tooling cannot reach a target page reliably. The primary workflow is `magicbrowse launch <url>` -> one or more `magicbrowse act "<goal>"` -> `magicbrowse close`. Run `magicbrowse doctor` once after install to verify the shared MagicPay gateway config.

- Use MagicBrowse to `magicbrowse launch <url>` and `magicbrowse act "summarize the visible pricing table"`.
- Use MagicBrowse to `magicbrowse launch <meta-search-url>` and `magicbrowse act "search one-way flights London → Lisbon next Tuesday and reach the passenger details page"`.
- Use MagicBrowse to `magicbrowse launch <support-url>` and `magicbrowse act "find the return-policy section and summarize the steps without submitting anything"`.

## Manual zip fallback

Use the manual path only if you want to manage the skill files yourself instead of letting OpenClaw install from ClawHub.

1. Download the public release archive for the current MagicBrowse skill version.
2. Extract the archive. You should get a folder named `magicbrowse/`.
3. Move that folder into `skills/` or `~/.openclaw/skills/`.
4. Restart OpenClaw if you already had a session open.

## Update or reinstall

1. Ask your agent to update the `magicbrowse` skill from ClawHub.
2. If prompted, provide your API key again and rerun `magicbrowse init <your-api-key>`.
3. Start a fresh OpenClaw session if the current session still does not see the updated skill.

## Troubleshooting

- **Skill not recognized by OpenClaw**: Start a fresh OpenClaw session so the runtime can rescan installed skills.
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
