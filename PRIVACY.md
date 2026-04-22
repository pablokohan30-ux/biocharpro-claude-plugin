# Privacy policy — biocharpro-claude-plugin

**Effective date**: April 2026.
**Last updated**: April 2026.

This document describes what data the Biochar Optimizer Pro Claude plugin
sends to biocharpro.io and how we handle it. It supplements (does not
replace) the [biocharpro.io privacy policy](https://biocharpro.io/legal/privacy).

## What this plugin does

The plugin registers a remote MCP server (`https://biocharpro.io/mcp`) with
Claude Code. When you ask Claude to invoke one of four tools
(`simulate_biochar`, `compare_feedstocks_batch`, `list_feedstocks`,
`extract_lab_analysis`), Claude sends a JSON-RPC request over HTTPS to that
endpoint, authenticated with the API key you configured.

## What data leaves your machine

Every tool call sends to `biocharpro.io`:

- The **tool name** invoked (`simulate_biochar`, etc.).
- The **tool arguments** you supplied or Claude inferred. For most tools
  this is numeric: temperature, residence time, feedstock IDs. For
  `extract_lab_analysis` the arguments include a **base64-encoded PDF file**
  that you chose to upload.
- Your **API key** in the `Authorization` header.
- Standard HTTP request metadata (timestamp, user agent, IP address).

The plugin does **not** send:

- Unrelated files from your filesystem.
- Conversation history from Claude.
- Your Anthropic account details.

## What we store

- **API key**: stored as a SHA-256 hash (we cannot recover the original key).
  Associated with your biocharpro.io account. Revocable from your dashboard
  at any time.
- **Last-used timestamp per key**: updated on every authenticated call, used
  to surface inactive keys in the dashboard.
- **Request counters** for rate-limit enforcement (in-memory, sliding
  1-minute window per key — not persisted).
- **Analytics**: pageviews + error reports for the biocharpro.io web app
  (PostHog EU, Sentry EU). These do NOT include MCP request payloads.

We do **not** persistently store:

- Tool inputs (temperature, feedstock IDs, custom feedstock data).
- Tool outputs (simulation results).
- PDF files sent to `extract_lab_analysis`. The PDF is forwarded to Gemini
  2.5 Flash for extraction and discarded immediately after.

## AI subprocessors

`extract_lab_analysis` sends your PDF and a system prompt to **Google Gemini
2.5 Flash** via the Google AI API. Google's data handling for that API is
governed by the [Google AI Terms of Service](https://policies.google.com/terms).
We do not opt this data into model training.

No other tool uses third-party AI subprocessors.

## Retention

- **API key hashes**: retained for as long as the key is active. Permanently
  deleted within 30 days of revocation.
- **Per-request logs** (stdout logs on the Fly.io machine): retained ~7 days
  for debugging and performance monitoring. Purged automatically.
- **Aggregate usage counters** (daily calls per user): retained indefinitely
  for billing and product analytics.

## Your rights

You can at any time:

- **Revoke an API key**: click the trash icon at
  [biocharpro.io/api](https://biocharpro.io/api).
- **Delete your account**: email the contact address on the dashboard.
  Account deletion removes all API keys and derived usage records within 30 days.
- **Export your projects**: use the JSON/PDF export on `/projects/:id`.

## Jurisdiction

biocharpro.io is operated by Emisiones Neutras S.A. (Argentina), with
primary hosting on Fly.io (us-east-1 via EU edge). EU users' GDPR rights
are honored via our EU-region hosting for analytics (PostHog EU, Sentry EU).

## Changes

Material changes to this policy will be communicated via the dashboard and
via the biocharpro.io newsletter (if subscribed). The top of this file always
reflects the current effective date.

## Contact

Questions about this policy: refer to the contact form at
[biocharpro.io/pricing#contact](https://biocharpro.io/pricing#contact).
