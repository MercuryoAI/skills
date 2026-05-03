# MagicBrowse Statuses And `finalMessage` Parsing

## `act` Result Shape

`magicbrowse act` returns:

- `status: completed | failed | max_steps | cancelled`
- `finalMessage: string` — the navigator's last summary
- `steps: number` — navigator step count
- `finalUrl: string | undefined` — last URL the navigator observed

CLI exit codes (from `statusToExitCode`):

| `status`    | exit code |
|-------------|-----------|
| `completed` | `0`       |
| `failed`    | `1`       |
| `max_steps` | `2`       |
| `cancelled` | `130`     |

A non-zero exit code is always a failure. A zero exit code is **not**
always a task success — `completed` is also returned when the planner
self-judges the task done, including at hard boundaries (auth walls,
captcha) where the human must take over.

## Parsing `finalMessage`

The `finalMessage` carries the navigator's prose summary. There is no
structured terminal-observation flag today — host orchestrators read
`finalMessage` for the actual outcome.

Common patterns to look for:

- **Auth wall.** Phrases such as "log in", "sign in", "authenticate",
  "session expired", or a finalMessage that ends with a request for
  user credentials. Treat as terminal: surface to the user, do not
  retry.
- **Captcha.** Phrases such as "captcha", "verify you are human",
  "challenge". Treat as terminal: surface to the user, do not script
  around.
- **Generic refusal.** The planner refused the task on safety
  grounds (entering credentials, payment fields, etc.). Treat as a
  contract violation by the orchestrator — switch skill (`magicpay`)
  or rephrase the goal to stop *at* the boundary.
- **Successful completion.** Concrete description of the terminal
  page that matches the goal's expected terminal state. The closer
  the original goal got to a checkable criterion (e.g. "ends on a
  page showing passenger fields"), the easier the host can confirm
  success automatically.
- **Plain failure.** `status: failed` with a reason describing the
  blocker (navigation error, page never loaded, element never
  appeared). Inspect `finalUrl` and re-`launch` if useful.

## When `status: max_steps`

The planner did not converge inside the step ceiling
(default 100, override with `--max-steps <n>`). Causes:

- Granule too large or vague — split on a page-change boundary or
  tighten the goal.
- Site requires repeated state-clearing (e.g. cookie banners that
  re-appear).
- Planner stuck in a loop the navigator cannot break.

Do not raise `--max-steps` as a default workaround; that hides
granularity bugs. Raise it only when you have a specific reason to
believe the task needs the headroom.

## When `status: cancelled`

The act was cancelled mid-run (typically SIGINT or programmatic
cancel). Session state on disk reflects the partial progress; the
next `act` continues from there.

## Layer-4 Primitive Results

`click`, `type`, `fill`, `select`, `press` emit a JSON action result
on stdout. Common blocked reasons:

- `target_not_found` — the `<target-id>` does not match anything in
  the most recent observe snapshot. Re-`observe` and retry.
- `unsupported_target` — the target is not the right kind for the
  action (e.g. `type` on a button). Re-read the observe snapshot
  for the correct kind.
- `click_failed` / `input_failed` / `select_failed` / `press_failed`
  — the action reached the page but the page rejected it. Re-`observe`
  to see the new state.

## Browser Session Errors

- `magicbrowse launch` failure — the runtime could not start Chrome.
  Check permissions, retry headless if `--headful` failed, or switch
  to `attach` if a host browser is available.
- `magicbrowse attach <endpoint>` failure — the CDP endpoint is
  unreachable or rejected the connection. Verify the endpoint and
  retry.
- `browser-status` reports the session unreachable mid-task —
  reconnect with `launch` or `attach`, then re-`observe`. Treat all
  pre-disconnect target-ids as stale.
