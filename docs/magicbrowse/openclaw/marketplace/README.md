# Install MagicBrowse from ClawHub

MagicBrowse gives OpenClaw a browser page-control fallback for real web pages: launch a fresh Chrome session, or attach to an explicitly approved CDP session, drive it with natural-language steps through `magicbrowse act`, and use deterministic `magicbrowse observe` plus `click`/`type`/`fill`/`select`/`press` primitives when single-element precision is required. It stops at login, identity, checkout, donation, subscription, and payment pages, returns a structured handoff for protected forms when available, and asks before consequential actions such as submit, post, book, buy, save, or delete unless a matching typed MagicPay approval already covers unchanged page facts.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- You have an API key for the magicbrowse gateway. Sign up at https://agents.mercuryo.io/signup if needed.
- The environment can launch a Chrome session. Use `magicbrowse attach` only for a private CDP endpoint explicitly approved for the current task.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicbrowse` skill from ClawHub in this workspace.
Treat MagicBrowse as a page-control fallback controller. Use it only when the runtime's own page-control tool cannot reach a target page reliably.
Ask me for my API key and run `magicbrowse init <your-api-key>`. The CLI uses the bundled default gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicbrowse` is missing, install or repair `@nuanu-ai/magicbrowse-cli`.
Verify the setup with `magicbrowse doctor`. The primary workflow is `magicbrowse launch <url>` -> one or more `magicbrowse act "<goal>"` -> `magicbrowse close`.
Use a fresh browser by default. Do not attach to an existing CDP endpoint, named profile, or user-data directory unless I explicitly approve that browser/session for this task.
Stop at login, identity, checkout, donation, subscription, and payment pages and return a Memory fill handoff for the orchestrator. Also ask me before submitting, posting, sending, saving, deleting, booking, buying, ordering, paying, accepting terms, or changing account data/settings, unless a matching typed MagicPay approval already covers the unchanged page facts.
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
2. If `magicbrowse doctor` still fails after `magicbrowse init`, inspect the persisted gateway config for a malformed key or wrong gateway URL.
3. Ask OpenClaw to use MagicBrowse on a small `magicbrowse launch <url>` -> `magicbrowse act "<goal>"` -> `magicbrowse close` workflow.

## Try a first task

Use MagicBrowse as a page-control fallback controller when the runtime's own page-control tool cannot reach a target page reliably. The primary workflow is `magicbrowse launch <url>` -> one or more `magicbrowse act "<goal>"` -> `magicbrowse close`. Run `magicbrowse doctor` once after install to verify the gateway config.

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
- **`magicbrowse` command not found**: Ask the agent to install or repair `@nuanu-ai/magicbrowse-cli`. If you need the manual fallback, run `npm i -g @nuanu-ai/magicbrowse-cli@latest`, then verify with `magicbrowse --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicbrowse init <your-api-key>`. The CLI uses the bundled default gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
- **`magicbrowse doctor` still fails after `magicbrowse init`**: Inspect the persisted gateway config for a malformed key or wrong gateway URL.
- **Browser launch is unavailable**: Use an environment that allows Chrome startup, or explicitly approve a private CDP endpoint for this task before using attach.
- **`magicbrowse act` returns `status: needs_handoff`**: Surface the message to the user. If the result includes `handoff.kind: memory_fill`, pass it to the orchestrator's MagicPay Memory fill workflow, then resume MagicBrowse with `handoff.resumeObjective` after the fill is complete. For a confirmed real CAPTCHA, use an external solver or user action, then `magicbrowse mark-captcha-resolved` before the next MagicBrowse step.
- **`magicbrowse act` returns `status: blocked` or `status: needs_approval`**: Ask for the missing input or exact approval instead of treating it as a runtime failure.
- **`magicbrowse act` returns `status: max_steps`**: The granule was too large or vague. Split the task on a page-change boundary or tighten the goal's expected terminal state.
- **The task reaches a login, identity, checkout, donation, subscription, or payment page**: Stop before Memory-managed data entry and return the handoff to the orchestrator.
- **The next step would submit, post, send, save, delete, book, buy, order, pay, or accept terms**: Stop and ask for explicit approval before executing that final action.
- **Concurrent workflows clobbering each other**: Set a distinct `MAGICBROWSE_HOME` per workflow; the default `~/.magicbrowse/current-session.json` is a singleton.

---

This guide is for MagicBrowse v0.1.18.
