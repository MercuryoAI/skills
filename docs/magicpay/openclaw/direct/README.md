# Install MagicPay in OpenClaw

<!-- magicpay-continuation-contract:v1 -->

MagicPay helps OpenClaw handle approved login, identity, checkout, donation, subscription, and payment pages. Once a usable or selected URL exists, start the MagicPay product workflow, then choose the browser process before page preparation. Use the OpenClaw built-in `browser` page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse. MagicPay plans Memory field fill, applies approved values without final submission, then returns continuation to the page-control tool that owns the browser.

## Before you start

- Node.js 18 or later is installed.
- OpenClaw is installed and working.
- OpenClaw's built-in `browser` page-control tool is available for normal page preparation and continuation when it can drive the attachable browser process.
- You have a MagicPay account and API key. Sign up at https://app.magiccard.ai/signup if needed.
- Browser-dependent MagicPay steps need either a MagicPay-launched browser child or an approved private CDP endpoint after `magicpay start-session`.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill in OpenClaw from https://github.com/nuanu-ai/skills/releases/tag/magicpay-v0.1.47 using `magicpay-openclaw-v0.1.47.zip`. Place the extracted `magicpay/` folder directly inside `skills/` or `~/.openclaw/skills/`.
Treat MagicPay as the helper for approved login, identity, checkout, donation, subscription, and payment workflows. The product workflow is the parent; browser work is a child resource when MagicPay needs browser-dependent execution.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@nuanu-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
A known x402 resource URL uses the installed native payment-operation workflow. Skip MagicSearch and never launch, attach, or commit a browser for that URL; bind the x402 purchase to one payment intent session.
For a purchase or booking intent with no usable checkout or booking URL, run `magicsearch query "<refined purchase prompt>" --choice-policy never --json` before generic web search or browser navigation. Keep currency inside the refined prompt and never add `--currency` to `magicsearch query`. If it returns `choice_required` for actual purchase options, resolve the user's choice and obtain the selected URL before `magicpay start-session`. Fall back explicitly only if MagicSearch is unavailable or cannot resolve a usable target.
With a usable or selected checkout or booking URL that is ordinary non-x402, skip MagicSearch and run `magicpay start-session` directly. If the query selects provider execution, follow the installed workflow's active-session exception for `magicsearch discover`.
For other MagicPay work, do not invoke MagicSearch; run `magicpay start-session` before browser preparation.
For browser work in OpenClaw, choose the browser process before page preparation: use the built-in `browser` page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse.
Do not start MagicBrowse as the first page-control tool in OpenClaw. MagicBrowse is fallback page-control when the OpenClaw built-in page-control tool cannot reliably reach, inspect, or continue the page.
When MagicPay needs browser-dependent work, bind the approved browser process inside the active product session with `magicpay launch` or approved `magicpay attach <cdp-url>` before preparing MagicPay-bound page state.
<!-- magicpay-continuation:v1 id=openclaw-direct-builder-plan-apply-1 action=plan-apply -->
After `magicpay plan-fill`, execute its exact returned `applyCommand`.
<!-- /magicpay-continuation:v1 -->
Then refresh and continue through the page-control tool that owns that browser.
Before a consequential action, get the matching typed MagicPay approval. For payments, follow the installed skill's closed normal-checkout `magicpay authorize-payment` shape exactly: keep country in the comparison record outside the command, never add `--country`, and keep every missing live fact unknown instead of using a sample or fallback. If an already open page is not CDP-reachable, keep its URL unknown and ask for the actual approved CDP endpoint or actual page URL; never write or launch a sample URL. After payment-card authorization finalizes, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand` before reporting the fields filled or ready. Never write, show, or execute a hand-written or sample `magicpay apply-fill`, `magicpay wait-request`, or `magicpay wait-memory`; use only exact returned command fields. Use `magicpay sign-message` for wallet message signing or `magicpay confirm-action` for a consequential action without a more specific typed command. After typed approval, proceed with exactly that action and stop if page facts changed.
`magicpay end-session` completes only the MagicPay product workflow. Browser lifecycle remains owned by OpenClaw's page-control path; use `magicpay close` only to close or clear the browser child inside the active product session.
When a real CAPTCHA is already visibly confirmed, run `magicpay solve-captcha [--timeout <s>]` directly without a confirming commit. Continue only for `fullyResolved: true` with `outcomeType: "resolved"`; a partial result must not be marked resolved or followed by commit/payment polling. After full resolution, get fresh visible page state from the current page-control owner and resume the documented current flow. Do not invent or run a page-state CLI such as `magicbrowse get-page-state`; that command is not documented. Obtain fresh visible state only through the existing page-control owner's documented continuation. If an old checkout plan is involved, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand`. No `magicpay observe` command is documented in this bundle. CAPTCHA clearance alone does not authorize `magicpay commit`; commit only at the normal matching-approval and current-live-facts boundary.
```

## What your agent should do

1. Download `magicpay-openclaw-v0.1.47.zip` from https://github.com/nuanu-ai/skills/releases/tag/magicpay-v0.1.47.
2. Extract the archive so the top-level folder is `magicpay/`.
3. Place `magicpay/` directly inside `skills/` or `~/.openclaw/skills/`.
4. If `magicpay` is missing, install or repair `@nuanu-ai/magicpay-cli`.
5. Request your API key and run `magicpay init <your-api-key>`; omit `--api-url` unless you need a non-default gateway.
6. Start a fresh OpenClaw session if the current session does not pick up the installed skill.

Supported skill locations:

- `skills/`
- `~/.openclaw/skills/`

## Verify the result

1. Open a new OpenClaw session in your workspace.
2. Ask OpenClaw to run `magicpay status`.
3. Ask OpenClaw to use MagicPay on a login, identity, checkout, donation, subscription, or payment page.
4. Once a usable or selected URL exists, confirm the agent starts the MagicPay product workflow with `magicpay start-session` before page preparation.
5. Confirm the browser process is chosen before page preparation: OpenClaw built-in page-control only when it drives the same private-CDP browser process that MagicPay will attach to, otherwise MagicPay-launched browser child controlled by an available controller such as MagicBrowse.
6. Confirm the Memory fill flow inside the active product workflow uses `magicpay launch` or approved `magicpay attach` before MagicPay-bound page preparation.
   <!-- magicpay-continuation:v1 id=openclaw-direct-openclaw-plan-apply-1 action=plan-apply -->
   After `magicpay plan-fill`, execute its exact returned `applyCommand`.
   <!-- /magicpay-continuation:v1 -->
   Then continue with the page-control owner and end with `magicpay end-session`.
7. If OpenClaw does not use MagicPay, make sure `magicpay/` sits directly inside a supported install location.

## Try a first task

Start with `magicpay status`. A known x402 resource URL uses the native payment-operation workflow: skip MagicSearch and never launch, attach, or commit a browser for it. For a purchase or booking intent with no usable checkout or booking URL, run `magicsearch query "<refined purchase prompt>" --choice-policy never --json`; keep currency inside the refined prompt and never add `--currency`. if it returns `choice_required` for actual purchase options, resolve the user's choice and obtain the selected URL before `magicpay start-session`. Retain the installed workflow's active-session exception for `magicsearch discover`. For other MagicPay work, do not invoke MagicSearch; run `magicpay start-session` before browser preparation. With a usable or selected checkout or booking URL that is ordinary non-x402, skip MagicSearch and run `magicpay start-session` directly. Choose the browser process before page preparation. In OpenClaw, use the built-in `browser` page-control tool, guided by browser-automation, only when it drives the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse. The current Memory fill path is `magicpay start-session` -> choose attachable browser process -> `magicpay launch` or approved `magicpay attach` before MagicPay-bound page preparation -> page-control tool drives that same browser -> returned-command handoff -> continuation with the page-control owner.
<!-- magicpay-continuation:v1 id=openclaw-direct-openclaw-plan-apply-2 action=plan-apply -->
After `magicpay plan-fill`, execute its exact returned `applyCommand`.
<!-- /magicpay-continuation:v1 -->
For payment authorization, follow the installed skill's closed normal-checkout `authorize-payment` command exactly; keep country in the comparison record outside the command, never add `--country`, and keep missing live facts unknown. Use `sign-message` for wallet message signing and `confirm-action` for other consequential actions without a more specific typed command. After typed approval, proceed with exactly that action and stop only if page facts changed. If an already open page is not CDP-reachable, keep its URL unknown and ask for the actual approved CDP endpoint or actual page URL; never write or launch a sample URL. After payment-card authorization finalizes, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand` before reporting the fields filled or ready. Never write, show, or execute a hand-written or sample `magicpay apply-fill`, `magicpay wait-request`, or `magicpay wait-memory`; use only exact returned command fields.

- With a usable or selected checkout URL that is ordinary non-x402, start `magicpay start-session`, confirm the OpenClaw built-in page-control tool is driving a private-CDP browser process, and attach that endpoint before checkout page preparation.
  <!-- magicpay-continuation:v1 id=openclaw-direct-openclaw-plan-apply-3 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then continue with the page-control owner.
- With a usable or selected URL, start `magicpay start-session`, launch a MagicPay browser child, and drive that same browser process with an available controller such as MagicBrowse until saved Memory values are required.
  <!-- magicpay-continuation:v1 id=openclaw-direct-openclaw-plan-apply-4 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then continue until the next approval boundary.
- With a usable donation or subscription checkout URL, start `magicpay start-session`, choose and bind an attachable browser process before MagicPay-bound page preparation.
  <!-- magicpay-continuation:v1 id=openclaw-direct-openclaw-plan-apply-5 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then use the matching typed approval before the final consequential confirmation.
- Use MagicPay on a payment authorization page after the OpenClaw built-in page-control tool shows the final facts; compare live amount, currency, recipient, recurrence, and country, then follow the installed skill's closed normal-checkout `magicpay authorize-payment` command exactly, keeping country outside the command, never adding `--country`, and keeping missing live facts unknown.
  <!-- magicpay-continuation:v1 id=openclaw-direct-openclaw-plan-apply-6 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then if amount, currency, recipient, recurrence, or country changes, obtain fresh matching approval before Pay/Submit.
- Use MagicPay on an identity-verification form after OpenClaw `browser` has reached it.
  <!-- magicpay-continuation:v1 id=openclaw-direct-openclaw-plan-stop-1 action=plan-unavailable-stop -->
  Stop if `magicpay plan-fill` cannot produce safe Memory matches.
  <!-- /magicpay-continuation:v1 -->
  Do not submit identity data without matching typed approval.

## Manual zip fallback

Use this fallback only if you want to manage the `magicpay/` folder yourself instead of asking OpenClaw to install it from https://github.com/nuanu-ai/skills/releases/tag/magicpay-v0.1.47 using `magicpay-openclaw-v0.1.47.zip`.

1. Download `magicpay-openclaw-v0.1.47.zip`.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into one of the supported install locations below.
4. Restart OpenClaw if you already had a session open.

## Update or reinstall

1. Ask your agent to download the newer archive for this OpenClaw runtime and replace the existing skill folder with the new `magicpay/` folder.
2. Keep the folder name exactly `magicpay` inside the same skills directory.
3. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
4. Start a fresh OpenClaw session after the replacement.

## Troubleshooting

- **Skill not recognized by OpenClaw**: Make sure the folder is named `magicpay` and sits directly inside `skills/` or `~/.openclaw/skills/`.
- **`magicpay` command not found**: The agent will try to install it automatically. If that fails, run `npm i -g @nuanu-ai/magicpay-cli@latest` and try again.
- **Missing API key**: Run `magicpay init <your-api-key>` or provide the key to OpenClaw and ask it to run init for you. Get a key at https://app.magiccard.ai/signup.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local MagicPay config file (`~/.magicpay/config.json` by default, or `$MAGICPAY_HOME/config.json` when isolated).
- **Memory plan unavailable**: Confirm the browser is still on the intended page.
  <!-- magicpay-continuation:v1 id=openclaw-direct-openclaw-plan-stop-2 action=plan-unavailable-stop -->
  Stop if `magicpay plan-fill` cannot produce safe Memory matches.
  <!-- /magicpay-continuation:v1 -->

- **OpenClaw uses MagicBrowse too early**: Use the OpenClaw built-in page-control tool when it can drive the same attachable browser process. MagicBrowse is fallback page-control when it cannot reliably reach, inspect, or continue that browser process.
- **Need to continue after Memory fill**: Continue through the OpenClaw built-in page-control tool from a refreshed page state.
- **No browser child**: Run `magicpay launch` or provide an approved private CDP endpoint for `magicpay attach` inside the active product session before browser-dependent MagicPay commands.

---

This guide is for MagicPay v0.1.47.
