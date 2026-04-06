# Install MagicPay from ClawHub

MagicPay lets OpenClaw attach to an already prepared browser page, bind a workflow session, discover a supported protected form, request approval for stored secrets, fill approved values, and submit the form safely.

## What you need before installing

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- You have a MagicPay account and API key. Sign up at https://magicpay.mercuryo.io if needed.
- Another tool or agent can prepare the browser and provide a CDP endpoint for the target page.

## Install

- Preferred path: ask your agent to install the `magicpay` skill from ClawHub in the current workspace.
- After install, ask the agent to request your MagicPay API key and run `magicpay init <your-api-key>`.
- ClawHub slug: `magicpay`
- Release page: https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.0

1. Ask your OpenClaw agent to install the `magicpay` skill from ClawHub in this workspace.
2. Ask the agent to request your MagicPay API key and run `magicpay init <your-api-key>`.
3. Ask the agent to confirm the install by running `magicpay --version` and `magicpay status`.
4. If the current OpenClaw session still does not see the skill after install or init, start a fresh OpenClaw session.

## Verify the install

1. Ask OpenClaw to confirm the skill is installed and run `magicpay --version` and `magicpay status`.
2. Ask OpenClaw to use MagicPay on a prepared login, identity, or payment form.
3. Confirm the agent already completed `magicpay init <your-api-key>` before the first protected-form task.

## Try it out

Let the agent handle the ClawHub install first, request your API key, and run init before you ask OpenClaw to use MagicPay on a prepared protected-form task.

- Use MagicPay to attach to the already open checkout page, run `magicpay find-form`, request approval for my stored card, fill it, and finish with `magicpay submit-form <fillRef>`.
- Use MagicPay to continue the prepared login page with stored credentials.
- Use MagicPay on the already prepared identity-verification form and stop if the form is ambiguous.

## Manual zip fallback

Use the manual path only if you want to manage the skill files yourself instead of letting OpenClaw install from ClawHub.

1. Download the public release archive for the current MagicPay skill version.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into `skills/` or `~/.openclaw/skills/`.
4. Restart OpenClaw if you already had a session open.

## Update or reinstall

1. Ask your agent to update the `magicpay` skill from ClawHub.
2. If required, provide your API key again and ask the agent to rerun `magicpay init <your-api-key>`.
3. If the current session still does not see the updated skill, start a fresh OpenClaw session.

## Troubleshooting

- **Skill not available after install**: Ask the agent to verify that `magicpay` is installed, then start a fresh OpenClaw session if the current one still does not see it.
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you want to do it yourself, run `npm i -g @mercuryo-ai/magicpay-cli`.
- **Missing API key**: Ask the agent to request your API key and run `magicpay init <your-api-key>` before retrying.
- **Need a manual fallback instead of ClawHub**: Use the direct-install guide from the public skills repo release and place `magicpay/` under an OpenClaw skills directory yourself.

---

This guide is for MagicPay v0.1.0.
