# Install MagicPay from ClawHub

MagicPay lets OpenClaw attach to an already prepared browser page, bind a workflow session, discover a supported protected form, request approval for stored secrets, and finish with `magicpay fill-secret <fillRef> <requestId>` on the normal path.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- Another tool or agent can prepare the browser and provide a CDP endpoint for the target page.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill from ClawHub in this workspace.
Ask me for my MagicPay API key and run `magicpay init <your-api-key>`.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay --version` and `magicpay status`; use `magicpay doctor` only if status still fails after init.
For the normal protected-form flow, finish with `magicpay fill-secret <fillRef> <requestId>`; use `magicpay submit-form <fillRef>` only for manual recovery.
```

## Install source

- Preferred path: ask your agent to install the `magicpay` skill from ClawHub in the current workspace.
- After install, ask the agent to request your MagicPay API key and run `magicpay init <your-api-key>`.
- ClawHub slug: `magicpay`

## What your agent should do

1. Install the `magicpay` skill from ClawHub in the current workspace.
2. Request your MagicPay API key and run `magicpay init <your-api-key>`.
3. Confirm the install by running `magicpay --version` and `magicpay status`.
4. Start a fresh OpenClaw session if the current session does not see the installed skill.

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

Use the manual path only if you want to manage the skill files yourself instead of letting OpenClaw install from ClawHub.

1. Download the public release archive for the current MagicPay skill version.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into `skills/` or `~/.openclaw/skills/`.
4. Restart OpenClaw if you already had a session open.

## Update or reinstall

1. Ask your agent to update the `magicpay` skill from ClawHub.
2. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
3. Start a fresh OpenClaw session if the current session still does not see the updated skill.

## Troubleshooting

- **Skill not recognized by OpenClaw**: Start a fresh OpenClaw session so the runtime can rescan installed skills.
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicpay-cli`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config. Use `doctor` for diagnostics only, not as a required first step.
- **Need to force a submit after fill**: Use `magicpay submit-form <fillRef>` only if `magicpay fill-secret <fillRef> <requestId>` reported that automatic submit was skipped or if you intentionally paused before submit on a fresh form snapshot.
- **No prepared browser context**: Open the target page in a browser first and provide a live CDP endpoint before invoking magicpay.

---

This guide is for MagicPay v0.1.2.
