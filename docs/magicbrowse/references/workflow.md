# MagicBrowse Worked Example

This walkthrough shows the primary workflow end-to-end: reach the
checkout page of an airline meta-search, then hand off to `magicpay`
for the protected step. The scenario assumes your own browser tooling
cannot drive the meta-search reliably.

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
`magicbrowse init <apiKey>` once, then re-run `doctor`. The persisted
config at `~/.magicpay/config.json` is shared with the `magicpay`
skill — one key, two skills.

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
{ "status": "completed", "finalMessage": "Payment page displayed with card number / expiry / CVV fields.", ... }
```

The goal explicitly reminds the planner not to enter payment details.
The planner and navigator already refuse credentials and payment
fields by default; the explicit instruction is a belt-and-braces note
for the host's own log.

> **Stop here.** The next step — typing into the payment fields — is
> the MagicPay boundary. Switching to the `magicpay` skill is the
> correct continuation.

## Hand off to `magicpay`

```text
$ magicpay attach <cdp-url-from-magicbrowse>
$ magicpay start-session checkout
$ magicpay find-form --purpose payment_card
$ magicpay resolve-form <fillRef>
```

`magicbrowse` does not call `close` here — the live CDP session is
handed to `magicpay`. Closing would tear down the browser and force
`magicpay` to re-attach to a fresh session.

## Cleanup

When the protected step completes (or the host abandons the task),
release the session:

```text
$ magicbrowse close
closed current magicbrowse session ...
```

## Failure Modes Encountered In This Scenario

- **Auth wall on the partner site.** If the partner OTA gates the
  passenger form behind a sign-in, `act` returns
  `status: completed` with `finalMessage` asking the user to log in.
  The orchestrator surfaces that to the user; it does not retry into
  the auth wall.
- **Captcha.** Same shape: `status: completed`, `finalMessage`
  describing the captcha. Surface to the user; do not script around
  it.
- **`status: max_steps`.** The granule was too large or too vague.
  Split it on a page-change boundary or tighten the goal's terminal
  state, then retry.
- **Stale meta-search state.** If the session has been open long
  enough for prices to drift, the host can `close` and re-`launch`
  to start clean instead of re-narrating into a follow-up `act`.

## What Not To Do In This Scenario

- ✗ A single `act "book the cheapest non-stop London → Lisbon and pay
  with my card"` — combines four strategic decisions into one task and
  crosses the MagicPay boundary.
- ✗ `magicbrowse run --url ... --goal ...` — the bundled `close`
  destroys the session that `magicpay` is about to attach to.
- ✗ Re-narrating prior context: `act "as we already searched, now
  pick result 2"` — sequential `act` calls preserve page state and
  planner memory; re-narration is a granularity smell.
- ✗ Driving payment fields with `type` or `fill` — even if the
  planner would refuse, the host must not attempt it; that is what
  `magicpay` is for.
