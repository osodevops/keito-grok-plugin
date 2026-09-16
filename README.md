<img src="assets/keito-icon.svg" alt="Keito" width="72" height="72">

# Keito for Grok Build

Track project work and AI usage in [Keito](https://keito.ai) without leaving Grok Build. Start and stop timers, log completed work and LLM usage, record expenses, and review project time through Keito's hosted MCP server. You sign in through your browser with OAuth.

This is the official Keito plugin, published by OSO DevOps Limited (trading as Keito). It is not made, endorsed, or verified by xAI.

## What's included

| Component | What it does |
|---|---|
| MCP server `keito` | Connects Grok Build to Keito's hosted server at `https://mcp.keito.ai` over Streamable HTTP. |
| Skill `keito-time-tracking` | Tells Grok how to use Keito safely: check the workspace, resolve projects before writing, confirm inferred writes, and avoid duplicate records. |

The plugin runs no local code. There are no hooks, scripts, binaries, or install steps.

## Requirements

- A Keito account with access to the clients, projects, and tasks you want Grok to use.
- Keito permissions for any write actions you plan to use (starting timers, creating time entries, logging expenses).
- [Grok Build](https://github.com/xai-org/grok-build) with plugin support.

## Install

**From the Grok Build marketplace:** open `/marketplace`, select **keito**, and press `i`.

**Directly from this repository**, pinned to a release commit:

```bash
grok plugin install osodevops/keito-grok-plugin@<commit-sha> --trust
```

Use the commit SHA published on the [Keito Grok Build integration page](https://keito.ai/docs/integrations/grok-build). Grok Build checks that the fetched commit matches.

Press `r` in the Plugins tab or start a new session to load the plugin.

## Connect your Keito account

1. Open `/mcps`, select **keito**, and press `i`.
2. Your browser opens Keito's sign-in page. Sign in and approve access.
3. Return to Grok Build. The Keito tools are now available.

You do not need an API key. Grok Build stores the OAuth tokens locally.

## Check it works

```text
Use Keito to show my current user, company, and running timer.
```

If Grok says no timer is running, the connection is working.

## Example requests

```text
List my Keito projects for client "Valueleaf Ltd" and the tasks under Website Platform.
```

```text
Start a Keito timer on Website Platform, task Development, with the note "Grok Build: API pagination".
```

```text
Stop my running Keito timer.
```

```text
Log 1.5 hours to Keito for today on Website Platform, task Documentation. Notes: "Wrote API usage guide."
```

```text
Log LLM usage to the Keito project Website Platform: provider anthropic, model claude-sonnet-4-6, 30000 input tokens, 12000 output tokens.
```

Keito calculates the cost for models in its price table. For any other model, include the amount in your request.

```text
Summarise my Keito time for 2026-09-01 to 2026-09-14 by project.
```

## Capabilities

Grok shows these tools as `keito__<tool name>`.

| Tool | Effect |
|---|---|
| `keito_server_info` | Read: server version and build. |
| `keito_whoami` | Read: your Keito user and current company. |
| `keito_list_clients` | Read: clients. |
| `keito_list_projects` | Read: projects. |
| `keito_list_tasks` | Read: tasks, optionally for one project. |
| `keito_list_time_entries` | Read: time entries with filters. |
| `keito_get_time_entry` | Read: one time entry. |
| `keito_get_running_timer` | Read: your running timer, if any. |
| `keito_list_expenses` | Read: expenses, including LLM usage. |
| `keito_report_team_time` | Read: team time report for a date range. |
| `keito_start_timer` | **Write:** starts a timer. Idempotent; won't replace a running timer unless asked. |
| `keito_stop_timer` | **Write:** stops a timer. Idempotent. |
| `keito_create_time_entry` | **Write:** creates a completed time entry. |
| `keito_update_time_entry` | **Write:** updates an existing time entry. |
| `keito_log_llm_usage` | **Write:** records an LLM usage expense against a project. |
| `keito_create_expense` | **Write:** creates a general expense. |

Grok can only act on data your Keito user can access.

### Not included

- **Deleting time entries.** OAuth clients never get this. Delete entries in the Keito web app.
- Invoices, estimates, and client or project administration.
- Google Calendar data, and time entries or reports built from it.

### Approvals

In Grok Build's default permission mode, each Keito tool call asks for your approval. In auto or always-approve mode, Grok may call tools without asking. The included skill tells Grok to confirm a write whenever it had to guess the project, date, duration, amount, or notes, but you are still responsible for your permission settings.

## Network access and credentials

| Host | Used for |
|---|---|
| `https://mcp.keito.ai` | MCP tool calls and OAuth protected-resource metadata. |
| `https://auth.keito.ai` | OAuth discovery, client registration, sign-in, consent, and token exchange. |

- **Credentials:** a Keito login, completed in your browser. There are no API keys, environment variables, or configuration files.
- **Authentication:** OAuth 2.0 authorization code flow with PKCE (S256) and dynamic client registration.
- **Local execution:** none. The plugin contains no executable files.

## Data handling

When Grok calls a Keito tool, Keito returns the data that tool needs from your authenticated workspace. That can include your user and company context, clients, projects, tasks, time entries, timers, expenses, team time reports, and LLM usage. That data is then processed by Grok Build under your account and agreement with xAI.

For each tool call, Keito's server keeps an audit record containing:

- Keito user and company IDs
- Tool name and a SHA-256 hash of the arguments
- Result status and latency
- MCP session and client information
- Source IP address

It does **not** log OAuth access or refresh tokens, API keys, cookies, or raw tool arguments.

Keito also keeps one connection record for each OAuth client connected to your account. The record holds the client ID, the most recent session ID, the client name and version, when it was first and last used, and when it was disconnected. It is what lets you review and disconnect the app in Keito.

Audit records are stored in AWS `eu-west-2`. Security audit records are kept for 90 days; billing reconciliation records for up to 13 months.

Keito does not use customer data to train AI models and does not send your data to a model provider on its own. See sections 2.5 and 5.3 of the [Keito Privacy Policy](https://keito.ai/privacy).

## Disconnect

1. **Revoke access in Keito:** in the Keito web app, open **Settings → Integrations → Connected AI apps** and disconnect **Grok Build**. Within about a minute Grok Build's Keito calls are rejected, including after a token refresh.
2. **Remove the plugin:** `grok plugin uninstall keito`.

Uninstalling the plugin alone does not revoke access on Keito's side, so do step 1 as well.

To use Keito from Grok Build again, click **Reconnect** in Keito, or sign in afresh. Grok keeps retrying its old sign-in rather than starting a new one, so first remove the `keito:https://mcp.keito.ai/` entry from `~/.grok/mcp_credentials.json`, then open `/mcps`, select **keito**, and press `i`. The new sign-in appears as a new **Grok Build** entry in Keito.

## Troubleshooting

| Symptom | What to do |
|---|---|
| Keito tools don't appear | Make sure the plugin is installed, enabled, and trusted (`grok plugin list`), then press `r` in the Plugins tab or start a new session. |
| Keito shows as needing authentication | Open `/mcps`, select **keito**, and press `i`. |
| `i` fails with `Auth failed … Auth required` after you disconnected Grok Build in Keito | Grok is retrying its old sign-in. Click **Reconnect** in Keito, or remove the `keito:https://mcp.keito.ai/` entry from `~/.grok/mcp_credentials.json` and press `i` again. |
| Opening `https://mcp.keito.ai` in a browser shows `401` | This is expected. The server requires OAuth. |
| Actions go to the wrong workspace | Ask Grok to run `keito_whoami`. Sign in again with the correct Keito account if needed. |
| "No matching project" | Ask Grok to list projects first, then use the exact name. |
| A timer won't start because one is running | Stop the running timer first, or explicitly ask Grok to replace it. |
| LLM usage is rejected for a missing amount | The model isn't in Keito's price table. Give the cost directly. |
| Sign-in keeps failing | Disconnect in Keito, reconnect from `/mcps`, and contact support with the time of the attempt. |

## Support

- Email: support@keito.ai
- Integration guide: https://keito.ai/docs/integrations/grok-build
- Security issues: see [SECURITY.md](SECURITY.md)
- [Privacy Policy](https://keito.ai/privacy) · [Terms of Service](https://keito.ai/terms)

## About this repository

Keito is developed and operated by OSO DevOps Limited (trading as Keito), company number 10889879. OSO DevOps Limited owns the `osodevops` GitHub organization, which publishes this plugin.

## License

[MIT](LICENSE)
