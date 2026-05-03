# MagicBrowse Command Guide

Full reference for the `magicbrowse` CLI. The skill workflow uses
`launch`, `act`, `observe`, `click`/`type`/`fill`/`select`/`press`, and
`close`. Setup uses `init` and `doctor`. Everything else is for
diagnostics or one-shot developer use.

The hard rules from `SKILL.md` apply to every command: use a fresh
browser by default, get explicit approval before using an existing
profile/CDP session, stop before consequential actions, and switch to
`magicpay` at protected forms.

## Setup And Readiness

### `magicbrowse init <apiKey> [--api-url <url>]`

Save the API key to `~/.magicpay/config.json` (the same file the
`magicpay` skill uses). When `--api-url` is provided, also stores the
gateway base URL there. Required for any LLM-backed `act` unless
`MAGICPAY_API_KEY` is set in the environment.

Exit codes: `0` on success, `1` if `<apiKey>` is missing.

### `magicbrowse doctor`

Verify the shared MagicPay gateway config and reachability. Use this
as the preflight before `launch` and `act`.

Exit codes: `0` if config is healthy, `1` if not.

### `magicbrowse browser-status`

Inspect live browser/page/runtime state. Use for diagnostics only.

Exit code: `0`.

## Session Lifecycle

### `magicbrowse launch [url] [--headful] [--profile <name>]`

Start an owned Chrome session and persist it as the current session.
The URL is **positional and optional**. Headless is the default;
`--headful` opts out. Advanced flags (`--user-data-dir`,
`--chrome-path`, `--user-agent`) accept overrides for non-default
Chrome layouts.

Prefer a fresh owned profile. Use `--profile` or `--user-data-dir`
only after the user explicitly approves that browser state for the
current task.

Exit code: `0`.

### `magicbrowse attach <cdp-url-or-ws-endpoint>`

Attach to an existing CDP browser as the current session. The
endpoint is **positional**, not a `--cdp-url` flag.

Only attach to a private endpoint that the user provided or explicitly
approved for the current task. Treat CDP endpoints as sensitive because
they inherit the authority of that browser session.

Exit codes: `0` on success, `1` if the endpoint is missing.

### `magicbrowse close`

Close or detach the current session. Always returns `0`.

## Natural-Language Browser Step

### `magicbrowse act "<prompt>" [--max-steps <n>] [--use-vision] [--format <fmt>]`

Run one natural-language browser step on the current session. The prompt is
**positional** (use double quotes for any prompt with spaces). `act`
does **not** take `--url`.

Use `act` for navigation, inspection, drafting, and preparation. If
the next step would submit a form, post or send content, accept terms,
change account data/settings, book, buy, order, delete, save, or
otherwise commit an irreversible/account-affecting action, stop and
ask for explicit approval. After approval, re-run `observe` and do
only the approved final action.

Options:

- `--max-steps <n>` — override the navigator step ceiling (default 100).
- `--use-vision` — include screenshots in the navigator's view. Use as
  a retry mode for the same goal only when the user is comfortable
  sending screenshots/page context for this workflow.
- `--format <human|text|json>` — output format. `json` emits JSON
  Lines suitable for an orchestrator. Default is `human`.

Exit codes (mapped from the act `status` field):

- `0` — `completed` (parse `finalMessage` to confirm task success;
  auth walls and captcha also return `completed`).
- `1` — `failed` or missing prompt or missing gateway config.
- `2` — `max_steps` (planner did not converge before the step ceiling).
- `130` — `cancelled` (e.g. SIGINT).

## Deterministic Primitives (Layer 4)

All take a `<target-id>` from the most recent `observe`. Target-ids
are bare integers from `[N]<type>text</type>` lines. They are scoped
to that single snapshot — re-run `observe` after any meaningful page
change.

### `magicbrowse observe`

Print the current public page snapshot (`plannerView`). Does not
accept a prompt or any positional argument. Stdout carries the human
snapshot; stderr carries a one-line summary of fillable target counts.

Exit codes: `0` on success, `1` if any positional argument is passed.

### `magicbrowse click <target-id>`

Click an observed action target.

### `magicbrowse type <target-id> <text>`

Type text into an observed text target. `<text>` is the rest of the
command line; quote it if it contains spaces or shell metacharacters.

### `magicbrowse fill <target-id> <value>`

Fill (replace) the current value of an observed text target.

### `magicbrowse select <target-id> <option-text>`

Select a native `<select>` option by visible label.

### `magicbrowse press <keys>`

Send a key chord to whatever the browser currently considers focused.
**Not target-scoped**: there is no way to address a specific element.
`click` an element first if focus matters. Examples: `Enter`,
`Tab`, `Control+A`.

All primitives:

- Return `0` on success and `1` on missing arguments.
- Emit a JSON action result on stdout (blocked or executed).
- Inherit the same approval boundary as `act`; do not click/press the
  final submit, save, delete, buy, book, accept, or send control unless
  the user explicitly approved that exact action and you re-observed
  the page first.

## Developer / One-Shot Compatibility

### `magicbrowse run --url <url> --goal "<goal>" [--use-vision]`

**Forbidden in orchestrated workflows.** Compatibility wrapper for
`launch + act + close`. The bundled `close` destroys session
continuity and persistent agent state, so it is documented here only
for one-shot developer use through `--help`. Hosts running a
multi-step skill workflow must use `launch [url] → act … act → close`.

Exit codes follow `act`.

## Environment Variables

- `MAGICPAY_API_KEY` — API key for the shared MagicPay gateway,
  alternative to `magicbrowse init`.
- `MAGICPAY_API_URL` — override the gateway base URL.
- `MAGICBROWSE_HOME` — root for per-run records and the singleton
  `current-session.json` (default `~/.magicbrowse`). Set distinct
  values per workflow for multi-tenant or parallel use.

## Updating The CLI

If `magicbrowse --version` is missing or outdated, run
`npm i -g @mercuryo-ai/magicbrowse-cli@0.0.3`, then verify with
`magicbrowse --version`.
