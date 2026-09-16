# Changelog

All notable changes to the installed Keito Grok Build plugin are recorded here. The plugin version tracks the files in this repository, not the Keito application or the hosted MCP server version.

Bumping `version` in `.grok-plugin/plugin.json` publishes a release: the Grok Build marketplace automation moves its pinned commit to the new version. Bump it only after the release checks pass.

## 1.0.0 — Unreleased

### Added

- Plugin manifest for Grok Build (`.grok-plugin/plugin.json`).
- HTTP MCP declaration for Keito's hosted server at `https://mcp.keito.ai` with browser OAuth (`.mcp.json`).
- `keito-time-tracking` skill with guidance for workspace checks, entity resolution, write confirmation, timers, time entries, LLM usage, expenses, and retries.
- README, security policy, and license.
