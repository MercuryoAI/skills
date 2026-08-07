# Install MagicPay from skills.sh

MagicPay helps compatible AI agents handle approved login, identity, checkout, donation, subscription, and payment pages. Start the MagicPay product workflow first with `magicpay start-session`, then choose the browser process before page preparation. Use the current agent's native page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse. When MagicPay needs browser-dependent work, bind that browser process as a child resource inside the active product workflow, then run `magicpay plan-fill` -> `magicpay apply-fill`. After MagicPay applies Memory fill, continue through the page-control tool that owns the browser. Consequential actions use typed approvals: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`; after typed approval, proceed with exactly that action and stop only if page facts changed.

## Before you start

- Node.js 18 or later is installed.
- Your AI agent can install Agent Skills through `npx skills`.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill from skills.sh using `npx skills add nuanu-ai/skills --skill magicpay --yes --copy`.
Treat MagicPay as the helper for approved login, identity, checkout, donation, subscription, and payment workflows. The product workflow is the parent; browser work is a child resource when MagicPay needs browser-dependent execution.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@nuanu-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
As soon as the task is identified as a MagicPay product workflow, run `magicpay start-session` before browser preparation.
For browser work, choose the browser process before page preparation: use the current agent's native page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse.
If native page-control is unavailable or cannot reliably reach, inspect, or continue the page, use MagicBrowse as fallback, while keeping `magicpay start-session` as the product workflow parent.
MagicBrowse is fallback page-control. Do not start MagicBrowse first when the current agent's native page-control tool can drive the same attachable browser process.
When MagicPay needs browser-dependent work, bind the approved browser process inside the active product session with `magicpay launch` or approved `magicpay attach <cdp-url>` before preparing MagicPay-bound page state, then run `magicpay plan-fill` and `magicpay apply-fill`. After MagicPay applies Memory fill, refresh and continue through the page-control tool that owns that browser.
Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment` for payments, `magicpay sign-message` for wallet message signing, or `magicpay confirm-action` for consequential actions without a more specific typed command. After typed approval, proceed with exactly that action and stop only if page facts changed.
`magicpay end-session` completes only the MagicPay product workflow. Browser lifecycle remains owned by the page-control owner; use `magicpay close` only to close or clear the browser child inside the active product session.
Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is confirmed present on the current browser child inside the active MagicPay product session.
```

## Install source

- Preferred path: install the universal MagicPay skill through `npx skills add nuanu-ai/skills --skill magicpay --yes --copy`.
- skills.sh source repository: `nuanu-ai/skills`
- After install, ask the agent to request your API key and run `magicpay init <your-api-key>`.

## What your agent should do

1. Install the `magicpay` skill with `npx skills add nuanu-ai/skills --skill magicpay --yes --copy`.
2. If the skills CLI asks for an agent target, choose the current agent or the universal `.agents/skills` path.
3. Request your API key and run `magicpay init <your-api-key>`.
4. Verify the install with `magicpay status`; MagicPay product workflows should start with `magicpay start-session`, and page preparation should use the current agent's native page-control only when it drives the same browser process MagicPay can attach to.
5. Start a fresh agent session if the current session does not see the installed skill.

## Verify the result

1. Ask the agent to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask the agent to start the MagicPay product workflow with `magicpay start-session` before page preparation.
4. Confirm the browser process is chosen before page preparation: current native page-control only when it drives the same private-CDP browser process that MagicPay will attach to, otherwise MagicPay-launched browser child controlled by an available controller such as MagicBrowse.
5. Confirm the Memory fill flow inside the active product workflow is `magicpay launch` or approved `magicpay attach` before MagicPay-bound page preparation -> page-control tool drives that same browser -> `magicpay plan-fill` -> `magicpay apply-fill` -> continuation through the page-control owner -> `magicpay end-session`.

## Try a first task

Start with `magicpay status`, then `magicpay start-session` as soon as the task is identified as a MagicPay product workflow. Choose the browser process before page preparation. Prefer the current agent's native page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse. If native page-control is unavailable or cannot reliably reach, inspect, or continue the page, use MagicBrowse as fallback. Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`.

- Start `magicpay start-session`, confirm the current native page-control tool is driving a private-CDP browser process, attach that endpoint before checkout page preparation, then run `magicpay plan-fill`, apply with `magicpay apply-fill`, and continue through the page-control owner.
- Start `magicpay start-session`, launch a MagicPay browser child, drive that same browser process with an available controller such as MagicBrowse until saved Memory values are required, run `magicpay plan-fill`, apply with `magicpay apply-fill`, then continue until the next approval boundary.
- If native page-control is unavailable or cannot drive the attachable browser process, use MagicBrowse as fallback to reach or continue the page in that same browser process.
- Use MagicPay on a payment authorization page after the page-control owner shows the final facts; collect visible amount, currency, recipient, optional description, and optional recurring status, then call `magicpay authorize-payment` and continue the approved Pay/Submit only while those facts stay unchanged.
- Use MagicPay on an identity-verification form after the page-control owner has reached it, and stop if `magicpay plan-fill` cannot produce safe Memory matches or before submitting identity data without a matching typed approval.

## Manual zip fallback

Use the manual path only if you want to manage the skill files yourself instead of letting the agent install from skills.sh.

1. Download the public release archive for the current MagicPay skill version.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into `.agents/skills/`, `~/.agents/skills/`, or the skill directory your agent scans.
4. Start a fresh agent session if you already had one open.

## Update or reinstall

1. Ask your agent to rerun `npx skills add nuanu-ai/skills --skill magicpay --yes --copy`, or use the skills CLI update command if the source is already tracked.
2. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
3. Start a fresh agent session if the current session still does not see the updated skill.

## Troubleshooting

- **Skill not recognized by General Agent**: Start a fresh General Agent session so the runtime can rescan installed skills.
- **`magicpay` command not found**: Ask the agent to install or repair `@nuanu-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @nuanu-ai/magicpay-cli@latest`, then verify with `magicpay --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config.
- **`magicpay plan-fill` cannot produce a safe Memory plan**: Confirm the browser is still on the intended page, then rerun `magicpay plan-fill`.
- **Missing product workflow**: Run `magicpay start-session` before `magicpay launch`, `magicpay attach`, or browser-dependent MagicPay commands.
- **No browser child**: Run `magicpay launch` or provide an approved private CDP endpoint for `magicpay attach` inside the active product session before browser-dependent MagicPay commands.
- **Confirmed CAPTCHA on the current page**: Use `magicpay solve-captcha [--timeout <s>]` on the current browser child inside the active product session, then continue the normal browser or MagicPay Memory fill flow.
- **Need to continue after Memory fill**: Continue through the page-control owner from a refreshed page state.

---

This guide is for MagicPay v0.1.43.
