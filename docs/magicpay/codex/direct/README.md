# Install MagicPay in Codex

<!-- magicpay-continuation-contract:v1 -->

MagicPay helps Codex handle approved login, identity, checkout, donation, subscription, and payment pages: once a usable or selected destination exists, start a MagicPay product workflow session, launch or attach an approved browser as a child resource, plan Memory field fill, apply approved values without final submission, and hand browser continuation back to the page-control owner.

## Before you start

- Node.js 18 or later is installed.
- Codex is installed and working.
- You have a MagicPay account and API key. Sign up at https://app.magiccard.ai/signup if needed.
- Browser-dependent steps need either a MagicPay-launched browser child or an approved private CDP endpoint after `magicpay start-session`.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill in Codex from https://github.com/nuanu-ai/skills/releases/tag/magicpay-v0.1.49 using `magicpay-codex-v0.1.49.zip`. Place the extracted `magicpay/` folder directly inside `$CODEX_HOME/skills/` or `~/.codex/skills/`.
Treat MagicPay as the helper for approved login, identity, checkout, donation, subscription, and payment workflows. The product workflow is the parent; the browser is a child resource.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@nuanu-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
A known x402 resource URL uses the installed native payment-operation workflow. Skip MagicSearch and never launch, attach, or commit a browser for that URL; bind the x402 purchase to one payment intent session.
For a purchase or booking intent with no usable checkout or booking URL, run `magicsearch query "<refined purchase prompt>" --choice-policy never --json` before generic web search or browser navigation. Keep currency inside the refined prompt and never add `--currency` to `magicsearch query`. If it returns `choice_required` for actual purchase options, resolve the user's choice and obtain the selected URL before `magicpay start-session`. Fall back explicitly only if MagicSearch is unavailable or cannot resolve a usable target.
With a usable or selected checkout or booking URL that is ordinary non-x402, skip MagicSearch and run `magicpay start-session` directly. If the query selects provider execution, follow the installed workflow's active-session exception for `magicsearch discover`.
For other MagicPay work, do not invoke MagicSearch; run `magicpay start-session` before browser preparation.
After `magicpay start-session`, run `magicpay launch` or `magicpay attach <cdp-url>`. Use only a browser/session I approve for this task, and keep my API key, local config, CDP endpoint, and Memory refs private.
The current Memory fill flow continues with `magicpay launch` or `magicpay attach` inside the active product session.
<!-- magicpay-continuation:v1 id=codex-direct-builder-plan-apply-1 action=plan-apply -->
After `magicpay plan-fill`, execute its exact returned `applyCommand`.
<!-- /magicpay-continuation:v1 -->
Then continue through the page-control owner.
Before a consequential action, get the matching typed MagicPay approval. For payments, follow the installed skill's closed normal-checkout `magicpay authorize-payment` shape exactly: keep country in the comparison record outside the command, never add `--country`, and keep every missing live fact unknown instead of using a sample or fallback. If an already open page is not CDP-reachable, keep its URL unknown and ask for the actual approved CDP endpoint or actual page URL; never write or launch a sample URL. After payment-card authorization finalizes, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand` before reporting the fields filled or ready. Never write, show, or execute a hand-written or sample `magicpay apply-fill`, `magicpay wait-request`, or `magicpay wait-memory`; use only exact returned command fields. Use `magicpay sign-message` for wallet message signing or `magicpay confirm-action` for a consequential action without a more specific typed command. After typed approval, proceed with exactly that action and stop if page facts changed.
`magicpay end-session` completes only the MagicPay product workflow. Browser lifecycle remains owned by the browser tool or orchestrator; use `magicpay close` only to close or clear the browser child inside the active product session.
When a real CAPTCHA is already visibly confirmed, run `magicpay solve-captcha [--timeout <s>]` directly without a confirming commit. Continue only for `fullyResolved: true` with `outcomeType: "resolved"`; a partial result must not be marked resolved or followed by commit/payment polling. After full resolution, get fresh visible page state from the current page-control owner and resume the documented current flow. Do not invent or run a page-state CLI such as `magicbrowse get-page-state`; that command is not documented. Obtain fresh visible state only through the existing page-control owner's documented continuation. If an old checkout plan is involved, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand`. No `magicpay observe` command is documented in this bundle. CAPTCHA clearance alone does not authorize `magicpay commit`; commit only at the normal matching-approval and current-live-facts boundary.
```

## What your agent should do

1. Download `magicpay-codex-v0.1.49.zip` from https://github.com/nuanu-ai/skills/releases/tag/magicpay-v0.1.49.
2. Extract the archive so the top-level folder is `magicpay/`.
3. Place `magicpay/` directly inside `$CODEX_HOME/skills/` or `~/.codex/skills/`.
4. If `magicpay` is missing, install or repair `@nuanu-ai/magicpay-cli`.
5. Request your API key and run `magicpay init <your-api-key>`; omit `--api-url` unless you need a non-default gateway.
6. Start a fresh Codex session if the current session does not pick up the installed skill.

Supported skill locations:

- `$CODEX_HOME/skills/`
- `~/.codex/skills/`

## Verify the result

1. Open a new Codex session in your repository.
2. Ask Codex to run `magicpay status`.
3. Ask Codex to use MagicPay on a login, identity, checkout, donation, subscription, or payment page.
4. Once a usable or selected URL exists, confirm the agent starts with `magicpay start-session`, then uses `magicpay launch` or `magicpay attach`.
   <!-- magicpay-continuation:v1 id=codex-direct-codex-plan-apply-1 action=plan-apply -->
   After `magicpay plan-fill`, execute its exact returned `applyCommand`.
   <!-- /magicpay-continuation:v1 -->
   Then end with `magicpay end-session`.
5. If Codex does not use MagicPay, make sure `magicpay/` sits directly inside a supported install location.

## Try a first task

Start with `magicpay status`. A known x402 resource URL uses the native payment-operation workflow: skip MagicSearch and never launch, attach, or commit a browser for it. For a purchase or booking intent with no usable checkout or booking URL, run `magicsearch query "<refined purchase prompt>" --choice-policy never --json`; keep currency inside the refined prompt and never add `--currency`. if it returns `choice_required` for actual purchase options, resolve the user's choice and obtain the selected URL before `magicpay start-session`. Retain the installed workflow's active-session exception for `magicsearch discover`. For other MagicPay work, do not invoke MagicSearch; run `magicpay start-session` before browser preparation. With a usable or selected checkout or booking URL that is ordinary non-x402, skip MagicSearch and run `magicpay start-session` directly. Launch or attach the approved browser inside that active product session before browser-dependent commands. The current Memory fill path is `launch/attach -> returned-command handoff -> browser-owner continuation`.
<!-- magicpay-continuation:v1 id=codex-direct-codex-plan-apply-2 action=plan-apply -->
After `magicpay plan-fill`, execute its exact returned `applyCommand`.
<!-- /magicpay-continuation:v1 -->
For payment authorization, follow the installed skill's closed normal-checkout `authorize-payment` command exactly; keep country in the comparison record outside the command, never add `--country`, and keep missing live facts unknown. Use `sign-message` for wallet message signing and `confirm-action` for other consequential actions without a more specific typed command. After typed approval, proceed with exactly that action and stop only if page facts changed. If an already open page is not CDP-reachable, keep its URL unknown and ask for the actual approved CDP endpoint or actual page URL; never write or launch a sample URL. After payment-card authorization finalizes, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand` before reporting the fields filled or ready. Never write, show, or execute a hand-written or sample `magicpay apply-fill`, `magicpay wait-request`, or `magicpay wait-memory`; use only exact returned command fields.

- With a usable or selected checkout URL that is ordinary non-x402, use MagicPay to run `magicpay start-session` and bind the approved checkout browser with `magicpay launch` or `magicpay attach`.
  <!-- magicpay-continuation:v1 id=codex-direct-codex-plan-apply-3 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then continue through the page-control owner.
- Use MagicPay to continue a login page after the product session exists and a browser child is bound.
  <!-- magicpay-continuation:v1 id=codex-direct-codex-plan-apply-4 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then let the page-control owner proceed until the next approval boundary.
- Use MagicPay on a donation or subscription checkout page; once its usable URL exists, start the product session and bind the browser child.
  <!-- magicpay-continuation:v1 id=codex-direct-codex-plan-apply-5 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then use the matching typed approval before the final consequential confirmation.
- Use MagicPay on a payment authorization page; compare live amount, currency, recipient, recurrence, and country, then follow the installed skill's closed normal-checkout `magicpay authorize-payment` command exactly, keeping country outside the command, never adding `--country`, and keeping missing live facts unknown.
  <!-- magicpay-continuation:v1 id=codex-direct-codex-plan-apply-6 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then if amount, currency, recipient, recurrence, or country changes, obtain fresh matching approval before Pay/Submit.
- Use MagicPay on an identity-verification form.
  <!-- magicpay-continuation:v1 id=codex-direct-codex-plan-stop-1 action=plan-unavailable-stop -->
  Stop if `magicpay plan-fill` cannot produce safe Memory matches.
  <!-- /magicpay-continuation:v1 -->
  Do not submit identity data without matching typed approval.

## Manual zip fallback

Use this fallback only if you want to manage the `magicpay/` folder yourself instead of asking Codex to install it from https://github.com/nuanu-ai/skills/releases/tag/magicpay-v0.1.49 using `magicpay-codex-v0.1.49.zip`.

1. Download `magicpay-codex-v0.1.49.zip`.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into one of the supported install locations below.
4. Restart Codex if you already had a session open.

## Update or reinstall

1. Ask your agent to download the newer archive for this Codex runtime and replace the existing skill folder with the new `magicpay/` folder.
2. Keep the folder name exactly `magicpay` inside the same skills directory.
3. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
4. Start a fresh Codex session after the replacement.

## Troubleshooting

- **Skill not recognized by Codex**: Make sure the folder is named `magicpay` and sits directly inside `$CODEX_HOME/skills/` or `~/.codex/skills/`.
- **`magicpay` command not found**: The agent will try to install it automatically. If that fails, run `npm i -g @nuanu-ai/magicpay-cli@latest` and try again.
- **Missing API key**: Run `magicpay init <your-api-key>` or provide the key to Codex and ask it to run init for you. Get a key at https://app.magiccard.ai/signup.
- **`magicpay status` still fails after init**: Run `magicpay doctor` to inspect the local MagicPay config file (`~/.magicpay/config.json` by default, or `$MAGICPAY_HOME/config.json` when isolated).
- **Memory plan unavailable**: Confirm the browser is still on the intended page.
  <!-- magicpay-continuation:v1 id=codex-direct-codex-plan-stop-2 action=plan-unavailable-stop -->
  Stop if `magicpay plan-fill` cannot produce safe Memory matches.
  <!-- /magicpay-continuation:v1 -->

- **Need to continue after Memory fill**: Continue through the page-control owner from a refreshed page state.
- **No browser child**: Run `magicpay launch` or provide an approved private CDP endpoint for `magicpay attach` inside the active product session before browser-dependent MagicPay commands.

---

This guide is for MagicPay v0.1.49.
