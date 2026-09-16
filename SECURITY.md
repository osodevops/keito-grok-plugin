# Security Policy

## Reporting a vulnerability

Report suspected vulnerabilities privately. Do not open a public issue.

- Use GitHub's **Report a vulnerability** button on this repository's **Security** tab, or
- Email **support@keito.ai** with the subject line `Security`.

Include the affected plugin version or commit, the Grok Build version, reproduction steps, and any request or correlation IDs. Do not include access tokens, API keys, or customer data.

## Scope

- The files in this repository.
- Keito's hosted MCP server at `https://mcp.keito.ai`.
- The OAuth flow at `https://auth.keito.ai` as used by this plugin.

Vulnerabilities in Grok Build itself should be reported to xAI.

## What this plugin contains

This plugin is declarative. It contains a manifest, one HTTP MCP server declaration, one instruction-only skill, documentation, a license, and a static SVG icon.

It does **not** contain:

- Executables, scripts, binaries, or bundled dependencies.
- Hooks, slash commands that run shell, or lifecycle scripts.
- Package manifests or install steps that download and run code.
- Secrets, API keys, or credentials.

Every file is plain text and can be reviewed directly. Marketplace listings pin this repository to a full 40-character commit SHA, and Grok Build verifies that commit after fetching it.

## Credentials and tokens

- Authentication uses OAuth 2.0 authorization code flow with PKCE (S256) and dynamic client registration. You never paste a Keito password or API key into Grok Build.
- Grok Build stores the resulting tokens on your machine (see Grok Build's MCP documentation). Keito never receives them outside the normal OAuth exchange.
- The hosted server does not log OAuth access tokens, refresh tokens, API keys, cookies, or raw tool arguments.
- Deleting time entries is not exposed to OAuth clients.

## Supported versions

Security fixes are released as a new plugin version. Only the latest released version is supported.
