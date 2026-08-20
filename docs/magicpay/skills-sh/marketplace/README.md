# Install MagicPay from skills.sh

<!-- magicpay-continuation-contract:v1 -->

MagicPay helps compatible AI agents set up MagicPay, connect local MagicPay config, generate card top-up links, and handle approved login, identity, checkout, donation, subscription, and payment pages from the Nuanu AI public skills repository through the universal skills.sh CLI. For first-time setup, the installed skill runs `magicpay setup next`, follows the returned instructions, verifies local config, and generates a hosted top-up link when the account is ready. For product workflows, once a usable or selected URL exists, start the MagicPay product workflow, then choose the browser process before page preparation. Use the current agent's native page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse.

## Before you start

- Node.js 18 or later is installed.
- Your AI agent can install Agent Skills through `npx skills`.
- You can receive the MagicPay one-time code at your email address.

## Ask your agent

Copy this request into your agent session:

```text
Install the `magicpay` skill from skills.sh using `npx skills add nuanu-ai/skills --skill magicpay --global --yes --copy`.
Treat MagicPay as the helper for approved login, identity, checkout, donation, subscription, and payment workflows. The product workflow is the parent; browser work is a child resource when MagicPay needs browser-dependent execution.
Ask me for my API key and run `magicpay init <your-api-key>`. The CLI uses the bundled default MagicPay gateway URL; pass `--api-url <url>` only for a non-default staging, self-hosted, or test gateway.
If `magicpay` is missing, install or repair `@nuanu-ai/magicpay-cli`.
Verify the setup with `magicpay status`. If it still fails after init, run `magicpay doctor`.
A known x402 resource URL uses the installed native payment-operation workflow. Skip MagicSearch and never launch, attach, or commit a browser for that URL; bind the x402 purchase to one payment intent session.
For a purchase or booking intent with no usable checkout or booking URL, run `magicsearch query "<refined purchase prompt>" --choice-policy never --json` before generic web search or browser navigation. Keep currency inside the refined prompt and never add `--currency` to `magicsearch query`. If it returns `choice_required` for actual purchase options, resolve the user's choice and obtain the selected URL before `magicpay start-session`. Fall back explicitly only if MagicSearch is unavailable or cannot resolve a usable target.
With a usable or selected checkout or booking URL that is ordinary non-x402, skip MagicSearch and run `magicpay start-session` directly. If the query selects provider execution, follow the installed workflow's active-session exception for `magicsearch discover`.
For other MagicPay work, do not invoke MagicSearch; run `magicpay start-session` before browser preparation.
For browser work, choose the browser process before page preparation: use the current agent's native page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse.
If native page-control is unavailable or cannot reliably reach, inspect, or continue the page, use MagicBrowse as fallback, while keeping `magicpay start-session` as the product workflow parent.
MagicBrowse is fallback page-control. Do not start MagicBrowse first when the current agent's native page-control tool can drive the same attachable browser process.
When MagicPay needs browser-dependent work, bind the approved browser process inside the active product session with `magicpay launch` or approved `magicpay attach <cdp-url>` before preparing MagicPay-bound page state.
<!-- magicpay-continuation:v1 id=skills-sh-marketplace-builder-plan-apply-1 action=plan-apply -->
After `magicpay plan-fill`, execute its exact returned `applyCommand`.
<!-- /magicpay-continuation:v1 -->
Then refresh and continue through the page-control tool that owns that browser.
Before a consequential action, get the matching typed MagicPay approval. For payments, follow the installed skill's closed normal-checkout `magicpay authorize-payment` shape exactly: keep country in the comparison record outside the command, never add `--country`, and keep every missing live fact unknown instead of using a sample or fallback. If an already open page is not CDP-reachable, keep its URL unknown and ask for the actual approved CDP endpoint or actual page URL; never write or launch a sample URL. After payment-card authorization finalizes, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand` before reporting the fields filled or ready. Never write, show, or execute a hand-written or sample `magicpay apply-fill`, `magicpay wait-request`, or `magicpay wait-memory`; use only exact returned command fields. Use `magicpay sign-message` for wallet message signing or `magicpay confirm-action` for a consequential action without a more specific typed command. After typed approval, proceed with exactly that action and stop if page facts changed.
`magicpay end-session` completes only the MagicPay product workflow. Browser lifecycle remains owned by the page-control owner; use `magicpay close` only to close or clear the browser child inside the active product session.
When a real CAPTCHA is already visibly confirmed, run `magicpay solve-captcha [--timeout <s>]` directly without a confirming commit. Continue only for `fullyResolved: true` with `outcomeType: "resolved"`; a partial result must not be marked resolved or followed by commit/payment polling. After full resolution, get fresh visible page state from the current page-control owner and resume the documented current flow. Do not invent or run a page-state CLI such as `magicbrowse get-page-state`; that command is not documented. Obtain fresh visible state only through the existing page-control owner's documented continuation. If an old checkout plan is involved, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand`. No `magicpay observe` command is documented in this bundle. CAPTCHA clearance alone does not authorize `magicpay commit`; commit only at the normal matching-approval and current-live-facts boundary.
```

## Install source

- Preferred path: install the universal MagicPay skill through `npx skills add nuanu-ai/skills --skill magicpay --global --yes --copy`.
- skills.sh source repository: `nuanu-ai/skills`
- After install, ask the agent to continue setup with `magicpay setup next`.

## What your agent should do

1. Install the `magicpay` skill with `npx skills add nuanu-ai/skills --skill magicpay --global --yes --copy`.
2. If the skills CLI asks for an agent target, choose the current agent or the universal `.agents/skills` path.
3. Request your API key and run `magicpay init <your-api-key>`.
4. Verify the install with `magicpay status`; once a usable or selected URL exists, MagicPay product workflows should start with `magicpay start-session`, and page preparation should use the current agent's native page-control only when it drives the same browser process MagicPay can attach to.
5. Start a fresh agent session if the current session does not see the installed skill.

## Verify the result

1. Open a fresh agent session.
2. Ask the agent to run `magicpay status`.
3. Ask the agent to use MagicPay on a login, identity, checkout, donation, subscription, or payment page.
4. Once a usable or selected URL exists, confirm the agent starts the MagicPay product workflow with `magicpay start-session` before page preparation.
5. Confirm the browser process is chosen before page preparation: current native page-control only when it drives the same private-CDP browser process that MagicPay will attach to, otherwise MagicPay-launched browser child controlled by an available controller such as MagicBrowse.
6. Confirm the Memory fill flow inside the active product workflow uses `magicpay launch` or approved `magicpay attach` before MagicPay-bound page preparation.
   <!-- magicpay-continuation:v1 id=skills-sh-marketplace-skills-sh-plan-apply-1 action=plan-apply -->
   After `magicpay plan-fill`, execute its exact returned `applyCommand`.
   <!-- /magicpay-continuation:v1 -->
   Then continue through the page-control owner and end with `magicpay end-session`.

## Try a first task

Start with `magicpay status`. A known x402 resource URL uses the native payment-operation workflow: skip MagicSearch and never launch, attach, or commit a browser for it. For a purchase or booking intent with no usable checkout or booking URL, run `magicsearch query "<refined purchase prompt>" --choice-policy never --json`; keep currency inside the refined prompt and never add `--currency`. if it returns `choice_required` for actual purchase options, resolve the user's choice and obtain the selected URL before `magicpay start-session`. Retain the installed workflow's active-session exception for `magicsearch discover`. For other MagicPay work, do not invoke MagicSearch; run `magicpay start-session` before browser preparation. With a usable or selected checkout or booking URL that is ordinary non-x402, skip MagicSearch and run `magicpay start-session` directly. Choose the browser process before page preparation. Prefer the current agent's native page-control tool when it can drive the same private-CDP browser process that MagicPay will attach to; otherwise launch the MagicPay browser child first and drive that same browser process through an available controller such as MagicBrowse. After Memory fill completes, continue through the page-control owner from fresh page state. For payment authorization, follow the installed skill's closed normal-checkout `magicpay authorize-payment` command exactly; keep country in the comparison record outside the command, never add `--country`, and keep missing live facts unknown. If an already open page is not CDP-reachable, keep its URL unknown and ask for the actual approved CDP endpoint or actual page URL; never write or launch a sample URL. After payment-card authorization finalizes, rerun `magicpay plan-fill` and execute exactly its returned `applyCommand` before reporting the fields filled or ready. Never write, show, or execute a hand-written or sample `magicpay apply-fill`, `magicpay wait-request`, or `magicpay wait-memory`; use only exact returned command fields.

- With a usable or selected checkout URL that is ordinary non-x402, start `magicpay start-session`, confirm the current native page-control tool is driving a private-CDP browser process, and attach that endpoint before checkout page preparation.
  <!-- magicpay-continuation:v1 id=skills-sh-marketplace-skills-sh-plan-apply-2 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then continue through the page-control owner.
- With a usable or selected URL, start `magicpay start-session`, launch a MagicPay browser child, and drive that same browser process with an available controller such as MagicBrowse until saved Memory values are required.
  <!-- magicpay-continuation:v1 id=skills-sh-marketplace-skills-sh-plan-apply-3 action=plan-apply -->
  After `magicpay plan-fill`, execute its exact returned `applyCommand`.
  <!-- /magicpay-continuation:v1 -->
  Then continue until the next approval boundary.
- If native page-control is unavailable or cannot drive the attachable browser process, use MagicBrowse as fallback to reach or continue the page in that same browser process.

## Manual zip fallback

Use the manual path only if you want to manage the skill files yourself instead of letting the agent install from skills.sh.

1. Download the public release archive for the current MagicPay skill version.
2. Extract the archive. You should get a folder named `magicpay/`.
3. Move that folder into `.agents/skills/`, `~/.agents/skills/`, or the skill directory your agent scans.
4. Start a fresh agent session if you already had one open.

## Update or reinstall

1. Ask your agent to rerun `npx skills add nuanu-ai/skills --skill magicpay --global --yes --copy`, or use the skills CLI update command if the source is already tracked.
2. If prompted, provide your API key again and rerun `magicpay init <your-api-key>`.
3. Start a fresh agent session if the current session still does not see the updated skill.

## Troubleshooting

- **Skill not available after install**: Ask the agent to verify that `magicpay` is installed, then start a fresh agent session if the current one still does not see it.
- **`magicpay` command not found**: Ask the agent to install or repair `@nuanu-ai/magicpay-cli@latest`. If you want to do it yourself, run `npm i -g @nuanu-ai/magicpay-cli@latest`.
- **Missing local config**: Ask the agent to use the installed MagicPay setup mode. It should run `magicpay setup next` and follow the returned instructions instead of guessing an email or UI-token branch.
- **Native page-control is unavailable**: Do not attempt unavailable browser tools. Use MagicBrowse as page-control fallback for a MagicPay-launched browser child, then keep `magicpay start-session` as the product workflow parent.
- **Need a manual fallback instead of skills.sh**: Use the direct-install guide from the public skills repo release and place `magicpay/` under the skill directory your agent scans.

---

This guide is for MagicPay v0.1.45.
