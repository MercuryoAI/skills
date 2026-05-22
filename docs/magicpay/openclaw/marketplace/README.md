# Install MagicPay from ClawHub

MagicPay helps OpenClaw handle approved login, identity, checkout, donation, subscription, and payment pages after the OpenClaw built-in browser, guided by browser-automation, has prepared the page. OpenClaw uses its native `browser` tool for navigation, snapshots, clicks, non-protected fields, screenshots, tab checks, and continuation. MagicPay starts only at the protected or sensitive step: `magicpay start-session` -> `magicpay launch` or approved `magicpay attach` -> `magicpay find-form` -> `magicpay resolve-form <fillRef>`. After MagicPay fills protected values, continue through the OpenClaw built-in browser. MagicBrowse only as fallback when the native browser cannot reliably reach, inspect, or continue the page. Consequential actions use typed approvals: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`; after typed approval, proceed with exactly that action and stop only if page facts changed.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- OpenClaw's built-in `browser` tool is available for normal page preparation and continuation.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- Browser-dependent MagicPay steps need either a MagicPay-launched browser child or an approved private CDP endpoint after `magicpay start-session`.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill from ClawHub in this workspace.
Treat MagicPay as the helper for approved login, identity, checkout, donation, subscription, and payment workflows. The product workflow is the parent; the browser is a child resource only for the protected step.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
For navigation and page preparation in OpenClaw, use the OpenClaw built-in browser guided by browser-automation first: open pages, read snapshots, click, fill non-protected fields, check tabs, take screenshots, and continue after MagicPay fills protected values.
Do not start MagicBrowse as the first browser path in OpenClaw. MagicBrowse only as fallback when the OpenClaw native browser cannot reliably reach, inspect, or continue the page.
Start `magicpay start-session` only when the task reaches a protected or sensitive MagicPay step, not for generic browsing or page preparation.
At the protected step, bind the approved browser inside that active product session with `magicpay launch` or approved `magicpay attach <cdp-url>`, then run `magicpay find-form` and `magicpay resolve-form <fillRef>`. After MagicPay fills the protected form, refresh and continue through the OpenClaw built-in `browser` tool. If MagicBrowse was explicitly used as fallback and returned a protected-form `handoff.resumeObjective`, use that as the next `magicbrowse act` goal.
Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment` for payments, `magicpay sign-message` for wallet message signing, or `magicpay confirm-action` for consequential actions without a more specific typed command. After typed approval, proceed with exactly that action and stop only if page facts changed.
`magicpay end-session` completes only the MagicPay product workflow. Browser lifecycle remains owned by OpenClaw's browser path; use `magicpay close` only to close or clear the browser child inside the active product session.
Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is confirmed present on the current browser child inside the active MagicPay product session.
```

## Install source

- Preferred path: ask your agent to install the `magicpay` skill from ClawHub in the current workspace.
- After install, ask the agent to request your API key and run `magicpay init <your-api-key>`.
- ClawHub slug: `magicpay`

## What your agent should do

1. Install the `magicpay` skill from ClawHub in the current workspace.
2. Request your API key and run `magicpay init <your-api-key>`.
3. Verify the install with `magicpay status`; normal page preparation should use the OpenClaw built-in browser first, and `magicpay start-session` should begin only at the protected or sensitive step.
4. Start a fresh OpenClaw session if the current session does not see the installed skill.

## Verify the result

1. Ask OpenClaw to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask OpenClaw to prepare the target page with the OpenClaw built-in browser and browser-automation before starting MagicPay for the protected step.
4. Confirm the protected-step flow is `magicpay start-session` -> `magicpay launch` or approved `magicpay attach` -> `magicpay find-form` -> `magicpay resolve-form <fillRef>` -> continuation with the OpenClaw built-in browser -> `magicpay end-session`.
5. Confirm MagicBrowse only as fallback: it should not be the first browser path unless the OpenClaw native browser cannot reliably reach, inspect, or continue the page.

## Try a first task

Do not start MagicBrowse as the first browser path in OpenClaw. First use the OpenClaw built-in browser, guided by browser-automation, to open and prepare the page. Start `magicpay start-session` only when the task reaches a protected or sensitive MagicPay step. Then use `magicpay launch` or approved `magicpay attach`, `magicpay find-form`, and `magicpay resolve-form <fillRef>`. After the protected fill completes, continue through the OpenClaw built-in browser. MagicBrowse only as fallback when the native browser cannot reliably reach, inspect, or continue the page. Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`.

- Use the OpenClaw built-in browser to reach the checkout and complete non-protected fields; at the protected step, run `magicpay start-session`, bind the approved browser with `magicpay launch` or approved `magicpay attach`, run `magicpay find-form`, fill with `magicpay resolve-form <fillRef>`, then continue with OpenClaw `browser`.
- Use the OpenClaw built-in browser to continue a login page until protected credentials are required; after the product session exists and a browser child is bound, fill with `magicpay resolve-form <fillRef>`, then let OpenClaw `browser` proceed until the next approval boundary.
- Use MagicPay on a donation or subscription checkout page only after OpenClaw `browser` reaches the protected form; bind the browser child, fill protected values, then use the matching typed approval before the final consequential confirmation.
- Use MagicPay on a payment authorization page after the OpenClaw built-in browser shows the final facts; collect visible amount, currency, recipient, optional description, and optional recurring status, then call `magicpay authorize-payment` and continue the approved Pay/Submit only while those facts stay unchanged.
- Use MagicPay on an identity-verification form after OpenClaw `browser` has reached it, and stop if `magicpay find-form` cannot confidently identify the form or before submitting identity data without a matching typed approval.

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
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicpay-cli@latest`, then verify with `magicpay --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config.
- **OpenClaw uses MagicBrowse too early**: Return to the OpenClaw built-in browser guided by browser-automation. MagicBrowse only as fallback when the native browser cannot reliably reach, inspect, or continue the page.
- **`magicpay find-form` cannot confidently identify the form**: Confirm the OpenClaw built-in browser is still on the intended page, then rerun `magicpay find-form`.
- **Missing product workflow**: Run `magicpay start-session` only when the task reaches the protected or sensitive MagicPay step, before `magicpay launch`, `magicpay attach`, or browser-dependent MagicPay commands.
- **No browser child**: Run `magicpay launch` or provide an approved private CDP endpoint for `magicpay attach` inside the active product session before browser-dependent MagicPay commands.
- **Confirmed CAPTCHA on the current page**: Use `magicpay solve-captcha [--timeout <s>]` on the current browser child inside the active product session, then continue the normal OpenClaw browser or MagicPay form flow.
- **Need to continue after protected fill**: Continue through the OpenClaw built-in browser. If MagicBrowse was explicitly used as fallback and returned a protected-form handoff, call `magicbrowse act` with `handoff.resumeObjective`; otherwise use the current visible page state in OpenClaw `browser` to choose the next narrow browser goal.

---

This guide is for MagicPay v0.1.26.
