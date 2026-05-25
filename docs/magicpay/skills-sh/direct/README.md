# Install MagicPay for a General Agent

MagicPay helps compatible AI agents handle approved login, identity, checkout, donation, subscription, and payment pages. Start the MagicPay product workflow first with `magicpay start-session`, then use the current agent's native browser automation for page preparation and continuation when it is available. MagicBrowse is fallback only when the native browser cannot reliably reach, inspect, or continue the page. When MagicPay needs browser-dependent work, bind the approved browser as a child resource inside the active product workflow with `magicpay launch` or approved `magicpay attach`, then run `magicpay find-form` -> `magicpay resolve-form <fillRef>`. After MagicPay fills protected values, continue through the browser owner. Consequential actions use typed approvals: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`; after typed approval, proceed with exactly that action and stop only if page facts changed.

## Before you start

- Node.js 18 or later is installed.
- Your AI agent supports Agent Skills from `.agents/skills/` or another configured skills directory.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- Browser-dependent MagicPay steps need either the current agent's native browser, a MagicPay-launched browser child, or an approved private CDP endpoint after `magicpay start-session`.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill for a compatible AI agent from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.31 using `magicpay-general-agent-v0.1.31.zip`. Place the extracted `magicpay/` folder directly inside `.agents/skills/` or `~/.agents/skills/`.
Treat MagicPay as the helper for approved login, identity, checkout, donation, subscription, and payment workflows. The product workflow is the parent; browser work is a child resource when MagicPay needs browser-dependent execution.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
As soon as the task is identified as a MagicPay product workflow, run `magicpay start-session` before browser preparation.
For browser work, use the current agent's native browser automation for page preparation, non-protected fields, and continuation after MagicPay fills protected values when that browser path is available.
If native browser automation is unavailable or cannot reliably reach, inspect, or continue the page, use MagicBrowse or a MagicPay-launched browser fallback, while keeping `magicpay start-session` as the product workflow parent.
MagicBrowse only as fallback. Do not start MagicBrowse as the first browser path when the current agent's native browser automation is available.
When MagicPay needs browser-dependent work, bind the approved browser inside the active product session with `magicpay launch` or approved `magicpay attach <cdp-url>`, then run `magicpay find-form` and `magicpay resolve-form <fillRef>`. After MagicPay fills the protected form, refresh and continue through the browser owner. If MagicBrowse was explicitly used as fallback and returned a protected-form `handoff.resumeObjective`, use that as the next `magicbrowse act` goal.
Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment` for payments, `magicpay sign-message` for wallet message signing, or `magicpay confirm-action` for consequential actions without a more specific typed command. After typed approval, proceed with exactly that action and stop only if page facts changed.
`magicpay end-session` completes only the MagicPay product workflow. Browser lifecycle remains owned by the browser owner; use `magicpay close` only to close or clear the browser child inside the active product session.
Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is confirmed present on the current browser child inside the active MagicPay product session.
```

## What your agent should do

1. Download `magicpay-general-agent-v0.1.31.zip` from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.31.
2. Extract the archive so the top-level folder is `magicpay/`.
3. Place `magicpay/` directly inside `.agents/skills/` or `~/.agents/skills/`.
4. If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
5. Request your API key and run `magicpay init <your-api-key>`; omit `--api-url` unless you need a non-default gateway.
6. Start a fresh General Agent session if the current session does not pick up the installed skill.

Supported skill locations:

- `.agents/skills/`
- `~/.agents/skills/`

## Verify the result

1. Ask the agent to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask the agent to start the MagicPay product workflow with `magicpay start-session` before page preparation.
4. Confirm the browser path uses the current agent's native browser automation when it is available, with MagicBrowse only as fallback.
5. Confirm the protected-step flow inside the active product workflow is `magicpay launch` or approved `magicpay attach` -> `magicpay find-form` -> `magicpay resolve-form <fillRef>` -> continuation through the browser owner -> `magicpay end-session`.

## Try a first task

Start with `magicpay status`, then `magicpay start-session` as soon as the task is identified as a MagicPay product workflow. Prefer the current agent's native browser automation to open and prepare the page, complete non-protected fields, and continue after MagicPay fills a protected form when that browser path is available. If native browser automation is unavailable or cannot reliably reach, inspect, or continue the page, use MagicBrowse or a MagicPay-launched browser fallback instead of attempting unavailable browser tools. MagicBrowse only as fallback. When MagicPay needs browser-dependent work, use `magicpay launch` or approved `magicpay attach`, `magicpay find-form`, and `magicpay resolve-form <fillRef>`. Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`.

- Start `magicpay start-session`, then use the current agent's native browser automation to reach the checkout and complete non-protected fields when available; at the protected step, bind the approved browser with `magicpay launch` or approved `magicpay attach`, run `magicpay find-form`, fill with `magicpay resolve-form <fillRef>`, then continue through the browser owner.
- Start `magicpay start-session`, then use the current agent's native browser automation to continue a login page until protected credentials are required; after a browser child is bound, fill with `magicpay resolve-form <fillRef>`, then let the browser owner proceed until the next approval boundary.
- If native browser automation is unavailable, use MagicBrowse only as fallback to reach or continue the page, then bind the browser child inside the active MagicPay product session for protected form work.
- Use MagicPay on a payment authorization page after the browser owner shows the final facts; collect visible amount, currency, recipient, optional description, and optional recurring status, then call `magicpay authorize-payment` and continue the approved Pay/Submit only while those facts stay unchanged.
- Use MagicPay on an identity-verification form after the browser owner has reached it, and stop if `magicpay find-form` cannot confidently identify the form or before submitting identity data without a matching typed approval.

## Manual zip fallback

Use this fallback only if you want to manage the `magicpay/` folder yourself instead of asking General Agent to install it from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.31 using `magicpay-general-agent-v0.1.31.zip`.

1. Download `magicpay-general-agent-v0.1.31.zip`.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into one of the supported install locations below.
4. Start a fresh agent session if you already had one open.

## Update or reinstall

1. Ask your agent to download the newer archive for this General Agent runtime and replace the existing skill folder with the new `magicpay/` folder.
2. Keep the folder name exactly `magicpay` inside the same skills directory.
3. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
4. Start a fresh General Agent session after the replacement.

## Troubleshooting

- **Skill not recognized by General Agent**: Make sure the folder sits directly inside `.agents/skills/` or `~/.agents/skills/`.
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicpay-cli@latest`, then verify with `magicpay --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config.
- **`magicpay find-form` cannot confidently identify the form**: Confirm the browser is still on the intended page, then rerun `magicpay find-form`.
- **Missing product workflow**: Run `magicpay start-session` before `magicpay launch`, `magicpay attach`, or browser-dependent MagicPay commands.
- **No browser child**: Run `magicpay launch` or provide an approved private CDP endpoint for `magicpay attach` inside the active product session before browser-dependent MagicPay commands.
- **Confirmed CAPTCHA on the current page**: Use `magicpay solve-captcha [--timeout <s>]` on the current browser child inside the active product session, then continue the normal browser or MagicPay form flow.
- **Need to continue after protected fill**: Continue through the browser owner. If MagicBrowse returned a protected-form handoff, call `magicbrowse act` with `handoff.resumeObjective`; otherwise use the current visible page state to choose the next narrow browser goal.

---

This guide is for MagicPay v0.1.31.
