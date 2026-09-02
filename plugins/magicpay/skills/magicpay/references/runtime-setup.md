# Runtime Setup: MagicPay host plugin

This plugin serves Codex, Claude Code, and OpenClaw. Use only the section
for the host you are running in; the canonical instructions in
[setup.md](setup.md) never name a host.

## Runtime Setup: Codex

Host-specific actions for the universal flow in [setup.md](setup.md). Only this
file names Codex commands; the canonical instructions stay host-neutral.

### Install

- Development channel: `codex plugin marketplace add nuanu-ai/skills --ref staging`
  registers marketplace `nuanu-skills-staging`, then
  `codex plugin add magicpay@nuanu-skills-staging`.
- Production channel, after promotion: the `nuanu-ai/skills` marketplace at its
  stable ref and selector `magicpay@nuanu-skills`.
- Keep exactly one installed `magicpay@…` selector.

### Connect

- Use the plugin's Connect action. If no model-visible Connect action is
  available but the bundled `magicpay` MCP registration exists, run the host's
  built-in `codex mcp login magicpay`. This is Codex connection management, not
  a MagicPay CLI or a second account login.
- Manual handoff when no Connect action can be initiated:
  **Plugins → MagicPay → Connect**.

### Catalog refresh

- Tell the user to click **New task** in the Codex sidebar so the host can load
  the plugin tools and reuse the completed connection. A reply in the current
  task is not a new task, and a new task is not the action that opens OAuth.

### Verify and disconnect

- `codex mcp list` shows the `magicpay` connection and its auth status.
- Disconnect: `codex mcp logout magicpay`. Remove:
  `codex plugin remove magicpay@<marketplace>`. Both are host-local and revoke
  nothing remotely.

## Runtime Setup: Claude Code

Host-specific actions for the universal flow in [setup.md](setup.md). Only this
file names Claude Code commands; the canonical instructions stay host-neutral.

### Install

- Development channel: `claude plugin marketplace add https://github.com/nuanu-ai/skills.git#staging`
  registers marketplace `nuanu-skills-staging` (skip when it is already
  registered), then `claude plugin install magicpay@nuanu-skills-staging`.
- Production channel, after promotion: `claude plugin marketplace add nuanu-ai/skills`,
  then `claude plugin install magicpay@nuanu-skills`.
- Keep exactly one installed `magicpay@…` selector. `claude plugin list --json`
  shows the installed selector, version, and bundled MCP server.

### Connect

- The bundled connection is named `plugin:magicpay:magicpay`.
- Interactive session: the user runs `/mcp`, selects `magicpay`, and
  authenticates; the secure browser OAuth window opens from there.
- Terminal: `claude mcp login plugin:magicpay:magicpay`. It requires an
  interactive terminal. An agent shell without one cannot start the login and
  must hand off to the user instead of trying workarounds.
- Manual handoff when no Connect action can be initiated:
  **/mcp → magicpay → Authenticate**.

### Catalog refresh

- A session that was already running when the plugin was installed or
  authenticated does not see the MagicPay tools until the user runs
  `/reload-plugins` or starts a new session. Neither action repeats OAuth, and
  neither is the action that opens OAuth.

### Verify and disconnect

- `claude mcp get plugin:magicpay:magicpay` reports `Connected` after OAuth.
- Disconnect: `claude mcp logout plugin:magicpay:magicpay`. Remove:
  `claude plugin uninstall magicpay@<marketplace>`. Both are host-local and
  revoke nothing remotely.

## Runtime Setup: OpenClaw

Host-specific actions for the universal flow in [setup.md](setup.md). Only this
file names OpenClaw commands; the canonical instructions stay host-neutral.

### Install

- Install the `magicpay` plugin through OpenClaw's native plugin manager. Its
  `openclaw.plugin.json` declares both the skill and the `magicpay` Streamable
  HTTP MCP server with `auth: oauth`; do not add a second MCP entry by hand.

### Connect

- `openclaw mcp login magicpay` opens the secure browser OAuth window and keeps
  the token in OpenClaw's owner-only store.
- Manual handoff when no Connect action can be initiated: run
  `openclaw mcp login magicpay`.

### Catalog refresh

- `openclaw mcp reload`. If tools still do not appear, publish the Gateway
  configuration or restart the process that owns the MCP clients. Neither
  repeats OAuth.

### Verify and disconnect

- `openclaw mcp doctor --probe` or `openclaw mcp status` confirms the
  connection.
- Per-requester OAuth links belong in private conversations; keep group-chat
  payment disabled unless the payer identity is private to the requester.
- Disconnect through OpenClaw's MCP connection management; it is host-local
  and revokes nothing remotely.
