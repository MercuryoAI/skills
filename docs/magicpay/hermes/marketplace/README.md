# Install MagicPay in Hermes from nuanu-ai/skills

<!-- magicpay-continuation-contract:v1 -->

MagicPay helps Hermes handle approved login, identity, checkout, donation, subscription, and payment pages from the nuanu-ai/skills GitHub hub/tap. Once a usable or selected URL exists, start the MagicPay product workflow, then choose the browser process before page preparation. Use Hermes native page-control automation when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse.

## Before you start

- Node.js 18 or later is installed.
- Hermes is installed and working with the `terminal` toolset available.
- You have a MagicPay account and API key. Sign up at https://app.magiccard.ai/signup if needed.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill from the nuanu-ai/skills GitHub tap in this Hermes workspace.
Treat MagicPay as the helper for approved login, identity, checkout, donation, subscription, and payment workflows. The product workflow is the parent; browser work is a child resource when MagicPay needs browser-dependent execution.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@nuanu-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
A known x402 resource URL uses the installed native payment-operation workflow. Skip MagicSearch and never launch, attach, or commit a browser for that URL; bind the x402 purchase to one payment intent session.
For a purchase or booking intent with no usable checkout or booking URL, run `magicsearch query "<refined purchase prompt>" --choice-policy never --json` before generic web search or browser navigation. Keep currency inside the refined prompt and never add `--currency` to `magicsearch query`. If it returns `choice_required` for actual purchase options, resolve the user's choice and obtain the selected URL before `magicpay start-session`. Fall back explicitly only if MagicSearch is unavailable or cannot resolve a usable target.
With a usable or selected checkout or booking URL that is ordinary non-x402, skip MagicSearch and run `magicpay start-session` directly. If the query selects provider execution, follow the installed workflow's active-session exception for `magicsearch discover`.
For other MagicPay work, do not invoke MagicSearch; run `magicpay start-session` before browser preparation.
For browser work in Hermes, choose the browser process before page preparation: use Hermes native page-control automation when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse.
If Hermes native page-control automation is unavailable or cannot reliably reach, inspect, or continue the page, do not attempt unavailable browser tools. Use MagicBrowse as fallback, while keeping `magicpay start-session` as the product workflow parent.
MagicBrowse is fallback page-control. Do not start MagicBrowse first when Hermes native page-control can drive the same attachable browser process.
When MagicPay needs browser-dependent work, bind the approved browser process inside the active product session with `magicpay launch` or approved `magicpay attach <cdp-url>` before preparing MagicPay-bound page state.
<!-- magicpay-continuation:v1 id=hermes-marketplace-builder-plan-apply-1 action=plan-apply -->
After `magicpay plan-fill`, execute its exact returned `applyCommand`.
<!-- /magicpay-continuation:v1 -->
Then refresh and continue through the page-control tool that owns that browser.
Before a consequential action, get the matching typed MagicPay approval. For payments, follow the installed skill's closed normal-checkout `magicpay authorize-payment` shape exactly: keep country in the comparison record outside the command, never add `--country`, and keep every missing live fact unknown instead of using a sample or fallback. If an already open page is not CDP-reachable, keep its URL unknown and ask for the actual approved CDP endpoint or actual page URL; never write or launch a sample URL. After payment-card authorization finalizes, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand` before reporting the fields filled or ready. Never write, show, or execute a hand-written or sample `magicpay apply-fill`, `magicpay wait-request`, or `magicpay wait-memory`; use only exact returned command fields. Use `magicpay sign-message` for wallet message signing or `magicpay confirm-action` for a consequential action without a more specific typed command. After typed approval, proceed with exactly that action and stop if page facts changed.
`magicpay end-session` completes only the MagicPay product workflow. Browser lifecycle remains owned by the Hermes page-control path or fallback page-control owner; use `magicpay close` only to close or clear the browser child inside the active product session.
When a real CAPTCHA is already visibly confirmed, run `magicpay solve-captcha [--timeout <s>]` directly without a confirming commit. Continue only for `fullyResolved: true` with `outcomeType: "resolved"`; a partial result must not be marked resolved or followed by commit/payment polling. After full resolution, get fresh visible page state from the current page-control owner and resume the documented current flow. Do not invent or run a page-state CLI such as `magicbrowse get-page-state`; that command is not documented. Obtain fresh visible state only through the existing page-control owner's documented continuation. If an old checkout plan is involved, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand`. No `magicpay observe` command is documented in this bundle. CAPTCHA clearance alone does not authorize `magicpay commit`; commit only at the normal matching-approval and current-live-facts boundary.
```

## Install source

- Preferred path: add `nuanu-ai/skills` as a Hermes GitHub tap, then install `magicpay` from that tap.
- Direct Hermes identifier: `nuanu-ai/skills/hermes/magicpay`
- After install, ask Hermes to request your API key and run `magicpay init <your-api-key>`.

## What your agent should do

1. Add the Nuanu AI skills tap with `hermes skills tap add nuanu-ai/skills` if it is not already configured.
2. Install the `magicpay` skill from `nuanu-ai/skills/hermes/magicpay`.
3. Request your API key and run `magicpay init <your-api-key>`.
4. Verify the install with `magicpay status`; once a usable or selected URL exists, MagicPay product workflows should start with `magicpay start-session`, and page preparation should use Hermes native page-control only when it drives the same browser process MagicPay can attach to.
5. Start a fresh Hermes session if the current session does not see the installed skill.

## Verify the result

1. Open a new Hermes session.
2. Ask Hermes to run `magicpay status`.
3. Ask Hermes to use MagicPay on a login, identity, checkout, donation, subscription, or payment page.
4. Once a usable or selected URL exists, confirm the agent starts the MagicPay product workflow with `magicpay start-session` before page preparation.
5. Confirm the browser process is chosen before page preparation: Hermes native page-control only when it drives the same private-CDP browser process that MagicPay will attach to, otherwise MagicPay-launched browser child controlled by an available controller such as MagicBrowse.
6. Confirm the Memory fill flow inside the active product workflow uses `magicpay launch` or approved `magicpay attach` before MagicPay-bound page preparation.
   <!-- magicpay-continuation:v1 id=hermes-marketplace-hermes-plan-apply-1 action=plan-apply -->
   After `magicpay plan-fill`, execute its exact returned `applyCommand`.
   <!-- /magicpay-continuation:v1 -->
   Then continue with the page-control owner and end with `magicpay end-session`.
7. If Hermes does not use MagicPay, make sure `magicpay/` sits directly inside a supported install location.

## Try a first task

Start with `magicpay status`. A known x402 resource URL uses the native payment-operation workflow: skip MagicSearch and never launch, attach, or commit a browser for it. For a purchase or booking intent with no usable checkout or booking URL, run `magicsearch query "<refined purchase prompt>" --choice-policy never --json`; keep currency inside the refined prompt and never add `--currency`. if it returns `choice_required` for actual purchase options, resolve the user's choice and obtain the selected URL before `magicpay start-session`. Retain the installed workflow's active-session exception for `magicsearch discover`. For other MagicPay work, do not invoke MagicSearch; run `magicpay start-session` before browser preparation. With a usable or selected checkout or booking URL that is ordinary non-x402, skip MagicSearch and run `magicpay start-session` directly. Choose the browser process before page preparation. Prefer Hermes native page-control automation only when it drives the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse. After Memory fill completes, continue through the page-control owner from fresh page state. For payment authorization, follow the installed skill's closed normal-checkout `authorize-payment` command exactly; keep country in the comparison record outside the command, never add `--country`, and keep missing live facts unknown. Use `sign-message` for wallet message signing and `confirm-action` for other consequential actions without a more specific typed command. After typed approval, proceed with exactly that action and stop only if page facts changed. If an already open page is not CDP-reachable, keep its URL unknown and ask for the actual approved CDP endpoint or actual page URL; never write or launch a sample URL. After payment-card authorization finalizes, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand` before reporting the fields filled or ready. Never write, show, or execute a hand-written or sample `magicpay apply-fill`, `magicpay wait-request`, or `magicpay wait-memory`; use only exact returned command fields.

- With a usable or selected checkout URL that is ordinary non-x402, start `magicpay start-session`, confirm Hermes native page-control is driving a private-CDP browser process, and attach that endpoint before checkout page preparation.
  <!-- magicpay-continuation:v1 id=hermes-marketplace-hermes-plan-apply-2 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then continue with the page-control owner.
- With a usable or selected URL, start `magicpay start-session`, launch a MagicPay browser child, and drive that same browser process with an available controller such as MagicBrowse until saved Memory values are required.
  <!-- magicpay-continuation:v1 id=hermes-marketplace-hermes-plan-apply-3 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then continue until the next approval boundary.
- If Hermes native page-control automation is unavailable or cannot drive the attachable browser process, use MagicBrowse as fallback to reach or continue the page in that same browser process.
- Use MagicPay on a payment authorization page after Hermes shows the final facts; compare live amount, currency, recipient, recurrence, and country, then follow the installed skill's closed normal-checkout `magicpay authorize-payment` command exactly, keeping country outside the command, never adding `--country`, and keeping missing live facts unknown.
  <!-- magicpay-continuation:v1 id=hermes-marketplace-hermes-plan-apply-4 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then if amount, currency, recipient, recurrence, or country changes, obtain fresh matching approval before Pay/Submit.
- Use MagicPay on an identity-verification form after Hermes native page-control automation has reached it.
  <!-- magicpay-continuation:v1 id=hermes-marketplace-hermes-plan-stop-1 action=plan-unavailable-stop -->
  Stop if `magicpay plan-fill` cannot produce safe Memory matches.
  <!-- /magicpay-continuation:v1 -->
  Do not submit identity data without matching typed approval.

## Manual zip fallback

Use the manual path only if you want to manage the skill files yourself instead of letting Hermes install from the nuanu-ai/skills GitHub tap.

1. Download the public release archive for the current MagicPay skill version.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into `~/.hermes/skills/`.
4. Start a fresh Hermes session if you already had one open.

## Update or reinstall

1. Ask your agent to update or reinstall the `magicpay` skill from the nuanu-ai/skills GitHub tap.
2. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
3. Start a fresh Hermes session if the current session still does not see the updated skill.

## Troubleshooting

- **Skill not available after install**: Ask Hermes to verify that `magicpay` is installed, then start a fresh Hermes session if the current one still does not see it.
- **`magicpay` command not found**: Ask Hermes to install or repair `@nuanu-ai/magicpay-cli@latest`. If you want to do it yourself, run `npm i -g @nuanu-ai/magicpay-cli@latest`.
- **Missing API key**: Ask Hermes to request your API key and run `magicpay init <your-api-key>` before retrying.
- **Hermes native page-control automation is unavailable**: Do not attempt unavailable browser tools. Use MagicBrowse as page-control fallback for a MagicPay-launched browser child, then keep `magicpay start-session` as the product workflow parent.
- **Hermes uses MagicBrowse too early**: Use Hermes native page-control automation when it can drive the same attachable browser process. MagicBrowse is fallback page-control when it cannot reliably reach, inspect, or continue that browser process.
- **Need a manual fallback instead of the Hermes GitHub tap**: Use the direct-install guide from the public skills repo release and place `magicpay/` under `~/.hermes/skills/` yourself.

---

This guide is for MagicPay v0.1.45.
