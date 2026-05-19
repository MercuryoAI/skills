# Install MagicPay from ClawHub

MagicPay helps OpenClaw handle approved login, identity, checkout, donation, subscription, and payment pages: attach to the approved browser session, bind a workflow session, discover the supported form, and fill it with values the user has approved through MagicPay. The main path is `magicpay find-form` -> `magicpay resolve-form <fillRef>`; after the protected fill completes, continue the page through the browser owner, using a MagicBrowse protected-form `handoff.resumeObjective` when one was returned. Consequential actions use typed approvals: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`. After typed approval, proceed with exactly that action and stop only if page facts changed. MagicPay completes its workflow with `magicpay end-session`, but browser cleanup remains owned by the browser tool or orchestrator that prepared the page.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- The relevant login, identity, checkout, donation, subscription, or payment page is already open in an approved browser session, or another tool can provide an approved private CDP endpoint for it.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill from ClawHub in this workspace.
Treat MagicPay as the helper for prepared login, identity, checkout, donation, subscription, and payment pages.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
Use only the prepared browser/session I approve for this task. Keep my API key, local config, CDP endpoint, and vault item ids private.
The main form flow is `magicpay find-form` -> `magicpay resolve-form <fillRef>`; after MagicPay fills the protected form, continue through the browser tool or orchestrator that prepared the page. If MagicBrowse returned a protected-form `handoff.resumeObjective`, use that as the next `magicbrowse act` goal. Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment` for payments, `magicpay sign-message` for wallet message signing, or `magicpay confirm-action` for consequential actions without a more specific typed command. After typed approval, proceed with exactly that action and stop only if page facts changed.
`magicpay end-session` completes only the MagicPay workflow. Browser lifecycle remains owned by the browser tool or orchestrator that prepared the page: clean up an owned disposable browser only after the overall task is done and never close an external or user-owned browser without my explicit teardown approval.
Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is confirmed present on the current MagicPay-attached browser session.
```

## Install source

- Preferred path: ask your agent to install the `magicpay` skill from ClawHub in the current workspace.
- After install, ask the agent to request your API key and run `magicpay init <your-api-key>`.
- ClawHub slug: `magicpay`

## What your agent should do

1. Install the `magicpay` skill from ClawHub in the current workspace.
2. Request your API key and run `magicpay init <your-api-key>`.
3. Verify the install with `magicpay status` and a prepared approved page.
4. Start a fresh OpenClaw session if the current session does not see the installed skill.

## Verify the result

1. Ask OpenClaw to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask OpenClaw to attach to an approved prepared login, identity, checkout, donation, subscription, or payment page, run `magicpay find-form`, fill it with `magicpay resolve-form <fillRef>`, and continue the page through the browser owner.

## Try a first task

Use MagicPay when the browser is already on the relevant login, identity, checkout, donation, subscription, or payment page and the user approved that browser/session for this task. The main form path is `magicpay find-form` -> `magicpay resolve-form <fillRef>`; after the protected fill completes, continue through the browser owner. If MagicBrowse returned a protected-form `handoff.resumeObjective`, use it as the next `magicbrowse act` goal. Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`. After typed approval, proceed with exactly that action and stop only if page facts changed. `magicpay end-session` ends the MagicPay workflow, then the browser owner decides whether to keep or clean up the browser.

- Use MagicPay to attach to the approved checkout page, run `magicpay find-form`, fill with `magicpay resolve-form <fillRef>`, then continue the checkout through the browser owner.
- Use MagicPay to continue the prepared login page; use `magicpay resolve-form <fillRef>`, then resume the login flow through the browser owner.
- Use MagicPay on the prepared donation or subscription checkout page; fill the protected form, then use the matching typed approval before the final consequential confirmation.
- Use MagicPay on a prepared payment authorization page; collect visible amount, currency, recipient, optional description, and optional recurring status, then call `magicpay authorize-payment` and continue the approved Pay/Submit only while those facts stay unchanged.
- Use MagicPay on the already prepared identity-verification form and stop if `magicpay find-form` cannot confidently identify the form or before submitting identity data without a matching typed approval.

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
- **`magicpay find-form` cannot confidently identify the form**: Confirm the browser is still on the intended page, then rerun `magicpay find-form`.
- **Confirmed CAPTCHA on the current page**: Use `magicpay solve-captcha [--timeout <s>]` on the current MagicPay-attached browser session, then continue the normal browser or MagicPay form flow.
- **Need to continue after protected fill**: Continue through the browser owner. If MagicBrowse returned a protected-form handoff, call `magicbrowse act` with `handoff.resumeObjective`; otherwise use the current visible page state to choose the next narrow browser goal.
- **No prepared browser context**: Open the target page in an approved browser session first and provide a private CDP endpoint before invoking magicpay.

---

This guide is for MagicPay v0.1.24.
