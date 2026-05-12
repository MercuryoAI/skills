# MagicBrowse Worked Example

This walkthrough shows the primary workflow end-to-end: reach the
checkout page of an airline meta-search, then stop at the payment
boundary and surface to the user. The scenario assumes your own
browser tooling cannot drive the meta-search reliably.

## Scenario

The user wants to book a one-way flight from London to Lisbon next
Tuesday for one passenger. The orchestrator has already chosen
`magicbrowse` as the fallback after its own browser tools failed to
search the site reliably.

## Preflight

```text
$ magicbrowse doctor
{ "success": true, ... }
```

Healthy. Proceed.

If `doctor` had failed, the orchestrator would ask the user for an
API key (sign-up at `https://agents.mercuryo.io/signup`) and run
`magicpay init <apiKey>` once, then re-run `doctor`. The persisted config
lives at `~/.magicpay/config.json` and is shared by MagicBrowse and MagicPay.

## Granule 1 — Search

Pre-place the session at the entry URL. Headless is the default; the
host has no reason to surface the browser to the user.

```text
$ magicbrowse launch https://www.kayak.com/flights
{ "session": { "id": "...", ... } }

$ magicbrowse act "Search one-way flights from London to Lisbon for next Tuesday for one adult passenger. End on the search results page."
... planner / navigator events ...
{ "status": "completed", "finalMessage": "Search results displayed for LON → LIS, Tue 2026-05-12, 1 adult.", ... }
```

The granule ends at a strategic decision point: which result to
choose. That decision belongs between `act` calls, not inside one.

## Granule 2 — Select a result

The orchestrator picks the first non-stop based on its own criteria
and asks `magicbrowse` to navigate to that result's checkout entry.

```text
$ magicbrowse act "Open the first non-stop result and proceed to the page that asks for passenger details."
... ...
{ "status": "completed", "finalMessage": "Reached passenger details page on partner site (gotogate.com).", ... }
```

Note: the planner navigated through a redirect from the meta-search to
a partner OTA. That is expected — do not add "stay on the same host"
to the goal; it would break almost every real booking flow.

## Granule 3 — Reach the protected boundary

```text
$ magicbrowse act "Fill the passenger first/last name and contact email with placeholder values, then proceed until the page shows the payment form. Do not enter any payment details yourself."
... ...
{ "status": "needs_handoff", "finalMessage": "Payment page displayed with card number / expiry / CVV fields. Payment entry is protected — surface to the user.", ... }
```

The goal explicitly reminds the planner not to enter payment details.
The planner and navigator already refuse credentials and payment
fields by default; the explicit instruction is a belt-and-braces note
for the host's own log.

> **Stop here.** The next step — typing into the payment fields — is
> the protected boundary. `magicbrowse` does not enter credentials,
> identity data, or payment data. Surface `finalMessage` to the user
> and let them decide what happens next; do not invent or placeholder
> protected values to push through.

## Surface and cleanup

The orchestrator passes `finalMessage` to the user along with the
current `finalUrl`. If the user closes the task here, release the
session:

```text
$ magicbrowse close
closed current magicbrowse session ...
```

If the next step is a MagicPay protected workflow on the current page, do not
close the browser before that handoff completes. Keep the browser available
for MagicPay. After MagicPay finishes with `magicpay end-session`, close only
if MagicBrowse launched an owned disposable browser for this task and the user
does not need to inspect or take over the page.

If the user instead takes over the browser themselves (or hands the
live CDP session to another tool they approved), the orchestrator
leaves the session open and lets `magicbrowse close` be called later
as teardown.

## Failure Modes Encountered In This Scenario

- **Auth wall on the partner site.** If the partner OTA gates the
  passenger form behind a sign-in, `act` returns
  `status: needs_handoff` with `finalMessage` asking the user to log in.
  The orchestrator surfaces that to the user; it does not retry into
  the auth wall.
- **CAPTCHA.** Same status: `needs_handoff`, with `finalMessage`
  describing the challenge. `magicbrowse` does not solve CAPTCHA. For a
  confirmed real CAPTCHA on the current approved browser session, use
  `magicpay solve-captcha [--timeout <s>]`; after a successful solve, run
  `magicbrowse mark-captcha-resolved` before the next `act`. Do not invent
  an answer or retry the same `act` against the wall.
- **Missing ordinary input.** `status: blocked` means MagicBrowse needs
  non-protected input or a different strategy before it can continue.
- **Final booking/payment action.** `status: needs_approval` means the
  page is ready for a consequential action and the user must approve
  the exact visible action before it is executed.
- **`status: max_steps`.** The granule was too large or too vague.
  Split it on a page-change boundary or tighten the goal's terminal
  state, then retry.
- **Stale meta-search state.** If the session has been open long
  enough for prices to drift, the host can `close` and re-`launch`
  to start clean instead of re-narrating into a follow-up `act`.

## What Not To Do In This Scenario

- ✗ A single `act "book the cheapest non-stop London → Lisbon and pay
  with my card"` — combines four strategic decisions into one task and
  crosses the protected-form boundary.
- ✗ `magicbrowse run --url ... --goal ...` — the bundled `close`
  destroys session continuity; a multi-step workflow must use
  `launch → act … act → close`.
- ✗ Re-narrating prior context: `act "as we already searched, now
  pick result 2"` — sequential `act` calls preserve page state and
  planner memory; re-narration is a granularity smell.
- ✗ Driving payment fields with `type` or `fill`, or placeholdering
  real card/identity data to push past the form. Stop at the boundary
  and surface to the user — never fabricate protected values.
