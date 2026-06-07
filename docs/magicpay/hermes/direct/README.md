# Install MagicPay in Hermes

MagicPay helps Hermes handle approved login, identity, checkout, donation, subscription, and payment pages. Start the MagicPay product workflow first with `magicpay start-session`, then prefer Hermes native browser automation for page preparation and continuation. MagicBrowse is fallback only when Hermes native browser automation is unavailable or cannot reliably reach, inspect, or continue the page. When MagicPay needs browser-dependent work, bind the approved browser as a child resource inside the active product workflow with `magicpay launch` or approved `magicpay attach`, then run `magicpay plan-fill` -> `magicpay apply-fill`. After MagicPay applies Memory fill, continue through Hermes native browser automation when available. Consequential actions use typed approvals: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`; after typed approval, proceed with exactly that action and stop only if page facts changed.

## Before you start

- Node.js 18 or later is installed.
- Hermes is installed and working with the `terminal` toolset available.
- Hermes native browser automation is enabled for the preferred browser path; if it is unavailable, use MagicBrowse or a MagicPay-launched browser fallback.
- You have a MagicPay account and API key. Sign up at https://agents.mercuryo.io/signup if needed.
- Browser-dependent MagicPay steps need either a MagicPay-launched browser child or an approved private CDP endpoint after `magicpay start-session`.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill in Hermes from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.37 using `magicpay-hermes-v0.1.37.zip`. Place the extracted `magicpay/` folder directly inside `~/.hermes/skills/`.
Treat MagicPay as the helper for approved login, identity, checkout, donation, subscription, and payment workflows. The product workflow is the parent; browser work is a child resource when MagicPay needs browser-dependent execution.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
As soon as the task is identified as a MagicPay product workflow, run `magicpay start-session` before browser preparation.
For browser work in Hermes, prefer Hermes native browser automation for page preparation, ordinary fields, and continuation after MagicPay applies Memory fill.
If Hermes native browser automation is unavailable or cannot reliably reach, inspect, or continue the page, do not attempt unavailable browser tools. Use MagicBrowse or a MagicPay-launched browser fallback, while keeping `magicpay start-session` as the product workflow parent.
MagicBrowse only as fallback. Do not start MagicBrowse as the first browser path when Hermes native browser automation is available.
When MagicPay needs browser-dependent work, bind the approved browser inside the active product session with `magicpay launch` or approved `magicpay attach <cdp-url>`, then run `magicpay plan-fill` and `magicpay apply-fill`. After MagicPay applies Memory fill, refresh and continue through Hermes native browser automation when available.
Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment` for payments, `magicpay sign-message` for wallet message signing, or `magicpay confirm-action` for consequential actions without a more specific typed command. After typed approval, proceed with exactly that action and stop only if page facts changed.
`magicpay end-session` completes only the MagicPay product workflow. Browser lifecycle remains owned by the Hermes browser path or fallback browser owner; use `magicpay close` only to close or clear the browser child inside the active product session.
Only call `magicpay solve-captcha [--timeout <s>]` when a real CAPTCHA is confirmed present on the current browser child inside the active MagicPay product session.
```

## What your agent should do

1. Download `magicpay-hermes-v0.1.37.zip` from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.37.
2. Extract the archive so the top-level folder is `magicpay/`.
3. Place `magicpay/` directly inside `~/.hermes/skills/`.
4. If `magicpay` is missing, install or repair `@mercuryo-ai/magicpay-cli`.
5. Request your API key and run `magicpay init <your-api-key>`; omit `--api-url` unless you need a non-default gateway.
6. Start a fresh Hermes session if the current session does not pick up the installed skill.

Supported skill locations:

- `~/.hermes/skills/`

## Verify the result

1. Ask Hermes to run `magicpay status`.
2. If `magicpay status` still fails after init, run `magicpay doctor` to inspect the local config.
3. Ask Hermes to start the MagicPay product workflow with `magicpay start-session` before page preparation.
4. Confirm the browser path uses Hermes native browser automation when it is available, with MagicBrowse only as fallback.
5. Confirm that if Hermes native browser automation is unavailable, the agent does not attempt unavailable browser tools and uses MagicBrowse or a MagicPay-launched browser fallback instead.
6. Confirm the Memory fill flow inside the active product workflow is `magicpay launch` or approved `magicpay attach` -> `magicpay plan-fill` -> `magicpay apply-fill` -> continuation with Hermes native browser automation when available -> `magicpay end-session`.

## Try a first task

Start with `magicpay status`, then `magicpay start-session` as soon as the task is identified as a MagicPay product workflow. Prefer Hermes native browser automation to open and prepare the page, complete ordinary fields, and continue after MagicPay applies Memory fill. If Hermes native browser automation is unavailable or cannot reliably reach, inspect, or continue the page, use MagicBrowse or a MagicPay-launched browser fallback instead of attempting unavailable browser tools. MagicBrowse only as fallback. When MagicPay needs browser-dependent work, use `magicpay launch` or approved `magicpay attach`, `magicpay plan-fill`, and `magicpay apply-fill`. Before a consequential action, get the matching typed MagicPay approval: `magicpay authorize-payment`, `magicpay sign-message`, or `magicpay confirm-action`.

- Start `magicpay start-session`, then use Hermes native browser automation to reach the checkout; at the sensitive step, bind the approved browser with `magicpay launch` or approved `magicpay attach`, run `magicpay plan-fill`, apply with `magicpay apply-fill`, then continue with Hermes native browser automation when available.
- Start `magicpay start-session`, then use Hermes native browser automation to continue a login page until saved Memory values are required; after a browser child is bound, run `magicpay plan-fill`, apply with `magicpay apply-fill`, then let Hermes native browser automation proceed until the next approval boundary.
- If Hermes native browser automation is unavailable, use MagicBrowse only as fallback to reach or continue the page, then bind the browser child inside the active MagicPay product session for Memory fill.
- Use MagicPay on a payment authorization page after Hermes shows the final facts; collect visible amount, currency, recipient, optional description, and optional recurring status, then call `magicpay authorize-payment` and continue the approved Pay/Submit only while those facts stay unchanged.
- Use MagicPay on an identity-verification form after Hermes native browser automation has reached it, and stop if `magicpay plan-fill` cannot produce safe Memory matches or before submitting identity data without a matching typed approval.

## Manual zip fallback

Use this fallback only if you want to manage the `magicpay/` folder yourself instead of asking Hermes to install it from https://github.com/MercuryoAI/skills/releases/tag/magicpay-v0.1.37 using `magicpay-hermes-v0.1.37.zip`.

1. Download `magicpay-hermes-v0.1.37.zip`.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into the supported install location below.
4. Start a fresh Hermes session if you already had one open.

## Update or reinstall

1. Ask your agent to download the newer archive for this Hermes runtime and replace the existing skill folder with the new `magicpay/` folder.
2. Keep the folder name exactly `magicpay` inside the same skills directory.
3. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
4. Start a fresh Hermes session after the replacement.

## Troubleshooting

- **Skill not recognized by Hermes**: Make sure the folder sits directly inside `~/.hermes/skills/`.
- **`magicpay` command not found**: Ask the agent to install or repair `@mercuryo-ai/magicpay-cli`. If you need the manual fallback, run `npm i -g @mercuryo-ai/magicpay-cli@latest`, then verify with `magicpay --version`.
- **Missing API key**: Sign up at https://agents.mercuryo.io/signup, then ask the agent to run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local config.
- **Hermes native browser automation is unavailable**: Do not attempt unavailable browser tools. Use MagicBrowse or a MagicPay-launched browser fallback, then keep `magicpay start-session` as the product workflow parent.
- **Hermes uses MagicBrowse too early**: Return to Hermes native browser automation when it is available. MagicBrowse only as fallback when the native browser cannot reliably reach, inspect, or continue the page.
- **`magicpay plan-fill` cannot produce a safe Memory plan**: Confirm the browser is still on the intended page, then rerun `magicpay plan-fill`.
- **Missing product workflow**: Run `magicpay start-session` as soon as the task is identified as a MagicPay product workflow, before browser preparation, `magicpay launch`, `magicpay attach`, or browser-dependent MagicPay commands.
- **No browser child**: Run `magicpay launch` or provide an approved private CDP endpoint for `magicpay attach` inside the active product session before browser-dependent MagicPay commands.
- **Confirmed CAPTCHA on the current page**: Use `magicpay solve-captcha [--timeout <s>]` on the current browser child inside the active product session, then continue the normal Hermes browser or MagicPay Memory fill flow.
- **Need to continue after Memory fill**: Continue through Hermes native browser automation when available from a refreshed page state.

---

This guide is for MagicPay v0.1.37.
