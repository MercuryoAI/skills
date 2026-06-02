# Install MagicPay in Claude Code

MagicPay helps Claude Code handle approved login, identity, checkout, donation, subscription, and payment pages: start the MagicPay product workflow with `magicpay start-session`, launch or attach an approved browser as a child resource, plan Memory field fill, and apply approved values without final submission. The main path is `magicpay start-session` -> `magicpay launch` or `magicpay attach` -> `magicpay plan-fill` -> `magicpay apply-fill`; after Memory fill completes, continue the page through the browser owner. Consequential actions use typed approvals: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`. After typed approval, proceed with exactly that action and stop only if page facts changed. MagicPay completes its workflow with `magicpay end-session`, while browser cleanup remains owned by the browser tool or orchestrator unless `magicpay close` is explicitly used for the browser child.

## Before you start

- Node.js 18 or later is installed.
- Claude Code is installed and working.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- Browser-dependent steps need either a MagicPay-launched browser child or an approved private CDP endpoint after `magicpay start-session`.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill in Claude Code from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.34 using `magicpay-claude-code-v0.1.34.zip`. Place the extracted `magicpay/` folder directly inside `.claude/skills/` or `~/.claude/skills/`.
Treat MagicPay as the helper for approved login, identity, checkout, donation, subscription, and payment workflows. The product workflow is the parent; the browser is a child resource.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
For normal product work, run `magicpay start-session` before `magicpay launch` or `magicpay attach <cdp-url>`. Use only a browser/session I approve for this task, and keep my API key, local config, CDP endpoint, and Memory refs private.
The current Memory fill flow is `magicpay start-session` -> `magicpay launch` or `magicpay attach` -> `magicpay plan-fill` -> `magicpay apply-fill`; after MagicPay applies Memory fill, continue through the browser tool or orchestrator that owns the browser. Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment` for payments, `magicpay sign-message` for wallet message signing, or `magicpay confirm-action` for consequential actions without a more specific typed command. After typed approval, proceed with exactly that action and stop only if page facts changed.
`magicpay end-session` completes only the MagicPay product workflow. Browser lifecycle remains owned by the browser tool or orchestrator; use `magicpay close` only to close or clear the browser child inside the active product session.
Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is confirmed present on the current browser child inside the active MagicPay product session.
```

## What your agent should do

1. Download `magicpay-claude-code-v0.1.34.zip` from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.34.
2. Extract the archive so the top-level folder is `magicpay/`.
3. Place `magicpay/` directly inside `.claude/skills/` or `~/.claude/skills/`.
4. If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
5. Request your API key and run `magicpay init <your-api-key>`; omit `--api-url` unless you need a non-default gateway.
6. Start a fresh Claude Code session if the current session does not pick up the installed skill.

Supported skill locations:

- `.claude/skills/`
- `~/.claude/skills/`

## Verify the result

1. Ask Claude Code to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask Claude Code to start a MagicPay product workflow with `magicpay start-session`, bind an approved browser child with `magicpay launch` or `magicpay attach`, run `magicpay plan-fill`, apply with `magicpay apply-fill`, and finish with `magicpay end-session`.

## Try a first task

Use MagicPay for approved login, identity, checkout, donation, subscription, or payment workflows. Start with `magicpay status`, then `magicpay start-session`; launch or attach the approved browser inside that active product session before browser-dependent commands. The current Memory fill path is `magicpay start-session` -> `magicpay launch` or `magicpay attach` -> `magicpay plan-fill` -> `magicpay apply-fill`; after Memory fill completes, continue through the browser owner. Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`. After typed approval, proceed with exactly that action and stop only if page facts changed. `magicpay end-session` ends the MagicPay workflow, then the browser owner decides whether to keep or clean up the browser.

- Use MagicPay to run `magicpay start-session`, bind the approved checkout browser with `magicpay launch` or `magicpay attach`, run `magicpay plan-fill`, apply with `magicpay apply-fill`, then continue the checkout through the browser owner.
- Use MagicPay to continue a login page after the product session exists and a browser child is bound; use `magicpay plan-fill` and `magicpay apply-fill`, then resume the login flow through the browser owner.
- Use MagicPay on a donation or subscription checkout page; start the product session first, bind the browser child, apply Memory fill, then use the matching typed approval before the final consequential confirmation.
- Use MagicPay on a payment authorization page; collect visible amount, currency, recipient, optional description, and optional recurring status, then call `magicpay authorize-payment` and continue the approved Pay/Submit only while those facts stay unchanged.
- Use MagicPay on an identity-verification form and stop if `magicpay plan-fill` cannot produce safe Memory matches or before submitting identity data without a matching typed approval.

## Manual zip fallback

Use this fallback only if you want to manage the `magicpay/` folder yourself instead of asking Claude Code to install it from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.34 using `magicpay-claude-code-v0.1.34.zip`.

1. Download `magicpay-claude-code-v0.1.34.zip`.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into one of the supported install locations below.
4. Restart Claude Code if you already had a session open.

## Update or reinstall

1. Ask your agent to download the newer archive for this Claude Code runtime and replace the existing skill folder with the new `magicpay/` folder.
2. Keep the folder name exactly `magicpay` inside the same skills directory.
3. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
4. Start a fresh Claude Code session after the replacement.

## Troubleshooting

- **Skill not recognized by Claude Code**: Make sure the folder sits directly inside `.claude/skills/` or `~/.claude/skills/`.
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicpay-cli@latest`, then verify with `magicpay --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config.
- **`magicpay plan-fill` cannot produce a safe Memory plan**: Confirm the browser is still on the intended page, then rerun `magicpay plan-fill`.
- **Missing product workflow**: Run `magicpay start-session` before `magicpay launch`, `magicpay attach`, or browser-dependent MagicPay commands.
- **No browser child**: Run `magicpay launch` or provide an approved private CDP endpoint for `magicpay attach` inside the active product session before browser-dependent MagicPay commands.
- **Confirmed CAPTCHA on the current page**: Use `magicpay solve-captcha [--timeout <s>]` on the current browser child inside the active product session, then continue the normal browser or MagicPay Memory fill flow.
- **Need to continue after Memory fill**: Continue through the browser owner from a refreshed page state.

---

This guide is for MagicPay v0.1.34.
