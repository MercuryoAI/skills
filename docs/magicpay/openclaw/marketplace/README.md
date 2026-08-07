# Install MagicPay from ClawHub

MagicPay helps OpenClaw handle approved login, identity, checkout, donation, subscription, and payment pages. Start the MagicPay product workflow first with `magicpay start-session`, then choose the browser process before page preparation. Use OpenClaw's built-in `browser` page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse. When MagicPay needs browser-dependent work, bind that browser process as a child resource inside the active product workflow, then run `magicpay plan-fill` -> `magicpay apply-fill`. After MagicPay applies Memory fill, continue through the page-control tool that owns the browser. Consequential actions use typed approvals: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`; after typed approval, proceed with exactly that action and stop only if page facts changed.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- OpenClaw's built-in `browser` page-control tool is available for normal page preparation and continuation when it can drive the attachable browser process.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- Browser-dependent MagicPay steps need either a MagicPay-launched browser child or an approved private CDP endpoint after `magicpay start-session`.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill from ClawHub in this workspace.
Treat MagicPay as the helper for approved login, identity, checkout, donation, subscription, and payment workflows. The product workflow is the parent; browser work is a child resource when MagicPay needs browser-dependent execution.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@nuanu-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
As soon as the task is identified as a MagicPay product workflow, run `magicpay start-session` before browser preparation.
For browser work in OpenClaw, choose the browser process before page preparation: use the built-in `browser` page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse.
Do not start MagicBrowse as the first page-control tool in OpenClaw. MagicBrowse is fallback page-control when the OpenClaw built-in page-control tool cannot reliably reach, inspect, or continue the page.
When MagicPay needs browser-dependent work, bind the approved browser process inside the active product session with `magicpay launch` or approved `magicpay attach <cdp-url>` before preparing MagicPay-bound page state, then run `magicpay plan-fill` and `magicpay apply-fill`. After MagicPay applies Memory fill, refresh and continue through the page-control tool that owns that browser.
Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment` for payments, `magicpay sign-message` for wallet message signing, or `magicpay confirm-action` for consequential actions without a more specific typed command. After typed approval, proceed with exactly that action and stop only if page facts changed.
`magicpay end-session` completes only the MagicPay product workflow. Browser lifecycle remains owned by OpenClaw's page-control path; use `magicpay close` only to close or clear the browser child inside the active product session.
Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is confirmed present on the current browser child inside the active MagicPay product session.
```

## Install source

- Preferred path: ask your agent to install the `magicpay` skill from ClawHub in the current workspace.
- After install, ask the agent to request your API key and run `magicpay init <your-api-key>`.
- ClawHub slug: `magicpay`

## What your agent should do

1. Install the `magicpay` skill from ClawHub in the current workspace.
2. Request your API key and run `magicpay init <your-api-key>`.
3. Verify the install with `magicpay status`; MagicPay product workflows should start with `magicpay start-session`, and page preparation should use OpenClaw's built-in page-control only when it drives the same browser process MagicPay can attach to.
4. Start a fresh OpenClaw session if the current session does not see the installed skill.

## Verify the result

1. Ask OpenClaw to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask OpenClaw to start the MagicPay product workflow with `magicpay start-session` before page preparation.
4. Confirm the browser process is chosen before page preparation: OpenClaw built-in page-control only when it drives the same private-CDP browser process that MagicPay will attach to, otherwise MagicPay-launched browser child controlled by an available controller such as MagicBrowse.
5. Confirm the Memory fill flow inside the active product workflow is `magicpay launch` or approved `magicpay attach` before MagicPay-bound page preparation -> page-control tool drives that same browser -> `magicpay plan-fill` -> `magicpay apply-fill` -> continuation with the page-control owner -> `magicpay end-session`.
6. Confirm MagicBrowse is fallback page-control: it should not be first when OpenClaw built-in page-control can drive the same attachable browser process.

## Try a first task

Start with `magicpay status`, then `magicpay start-session` as soon as the task is identified as a MagicPay product workflow. Choose the browser process before page preparation. In OpenClaw, use the built-in `browser` page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse. Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`.

- Start `magicpay start-session`, confirm OpenClaw built-in page-control is driving a private-CDP browser process, attach that endpoint before checkout page preparation, then run `magicpay plan-fill`, apply with `magicpay apply-fill`, and continue with the page-control owner.
- Start `magicpay start-session`, launch a MagicPay browser child, drive that same browser process with an available controller such as MagicBrowse until saved Memory values are required, run `magicpay plan-fill`, apply with `magicpay apply-fill`, then continue until the next approval boundary.
- Start `magicpay start-session` for a donation or subscription checkout workflow, choose and bind an attachable browser process before MagicPay-bound page preparation, apply planned values, and use the matching typed approval before the final consequential confirmation.
- Use MagicPay on a payment authorization page after the OpenClaw built-in page-control tool shows the final facts; collect visible amount, currency, recipient, optional description, and optional recurring status, then call `magicpay authorize-payment` and continue the approved Pay/Submit only while those facts stay unchanged.
- Use MagicPay on an identity-verification form after OpenClaw `browser` has reached it, and stop if `magicpay plan-fill` cannot produce safe Memory matches or before submitting identity data without a matching typed approval.

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
- **`magicpay` command not found**: Ask the agent to install or repair `@nuanu-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @nuanu-ai/magicpay-cli@latest`, then verify with `magicpay --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config.
- **OpenClaw uses MagicBrowse too early**: Use the OpenClaw built-in page-control tool when it can drive the same attachable browser process; MagicBrowse is fallback page-control when it cannot.
- **`magicpay plan-fill` cannot produce a safe Memory plan**: Confirm the OpenClaw built-in page-control tool is still on the intended page, then rerun `magicpay plan-fill`.
- **Missing product workflow**: Run `magicpay start-session` as soon as the task is identified as a MagicPay product workflow, before browser preparation, `magicpay launch`, `magicpay attach`, or browser-dependent MagicPay commands.
- **No browser child**: Run `magicpay launch` or provide an approved private CDP endpoint for `magicpay attach` inside the active product session before browser-dependent MagicPay commands.
- **Confirmed CAPTCHA on the current page**: Use `magicpay solve-captcha [--timeout <s>]` on the current browser child inside the active product session, then continue the normal OpenClaw browser or MagicPay Memory fill flow.
- **Need to continue after Memory fill**: Continue through the OpenClaw built-in page-control tool from a refreshed page state.

---

This guide is for MagicPay v0.1.43.
