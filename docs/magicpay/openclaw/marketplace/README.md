# Install MagicPay from ClawHub

MagicPay gives OpenClaw a protected-step workflow for approved prepared browser pages: attach to the approved browser session, bind a workflow session, discover the supported login, identity, or payment form, and fill it for review with values the user has approved through MagicPay. The main flow is `magicpay find-form` -> `magicpay resolve-form <fillRef> --no-submit` -> explicit user approval -> `magicpay submit-form <fillRef>`; protected actions use `magicpay run-action` only after the user approves the capability and params.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The relevant login, identity, or payment page is already open in an approved browser session, or another tool can provide an approved private CDP endpoint for it.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill from ClawHub in this workspace.
Treat MagicPay as the protected-form layer for a prepared login, identity, or payment page.
Ask me for my API key and run `magicpay init <your-api-key>`.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
Use only the prepared browser/session I approve for this task. Keep my API key, local config, CDP endpoint, and vault item ids private.
The main form flow is `magicpay find-form` -> `magicpay resolve-form <fillRef> --no-submit` -> ask me to approve the exact site/merchant, action, and visible amount or data -> `magicpay submit-form <fillRef>` only if I approve. Use `magicpay run-action <capability> --params-json <json>` only after I approve the capability and params.
```

## Install source

- Preferred path: ask your agent to install the `magicpay` skill from ClawHub in the current workspace.
- After install, ask the agent to request your API key and run `magicpay init <your-api-key>`.
- ClawHub slug: `magicpay`

## What your agent should do

1. Install the `magicpay` skill from ClawHub in the current workspace.
2. Request your API key and run `magicpay init <your-api-key>`.
3. Verify the install with `magicpay status` and a prepared protected-form task.
4. Start a fresh OpenClaw session if the current session does not see the installed skill.

## Verify the result

1. Ask OpenClaw to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask OpenClaw to attach to an approved prepared protected form, run `magicpay find-form`, and fill for review with `magicpay resolve-form <fillRef> --no-submit`.

## Try a first task

Use MagicPay when the browser is already on the relevant login, identity, or payment form and the user approved that browser/session for this task. The main form flow is `magicpay find-form` -> `magicpay resolve-form <fillRef> --no-submit` -> explicit user approval -> `magicpay submit-form <fillRef>`. Use `magicpay run-action <capability> --params-json <json>` only after the user approves the capability and params.

- Use MagicPay to attach to the approved checkout page, run `magicpay find-form`, and fill for review with `magicpay resolve-form <fillRef> --no-submit`; ask before submit.
- Use MagicPay to continue the prepared login page; use `magicpay resolve-form <fillRef> --no-submit` and ask before final login submit.
- Use MagicPay on the already prepared identity-verification form and ask me before picking between multiple supported forms or submitting identity data.

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
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicpay-cli@0.1.10`, then verify with `magicpay --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config.
- **`magicpay find-form` cannot confidently identify the form**: Confirm the browser is still on the intended page, then rerun `magicpay find-form`.
- **Need to submit after resolution**: Use `magicpay submit-form <fillRef>` only after the user approves the current site/merchant, action, and visible amount or data on a fresh form snapshot.
- **No prepared browser context**: Open the target page in an approved browser session first and provide a private CDP endpoint before invoking magicpay.

---

This guide is for MagicPay v0.1.13.
