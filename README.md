# Biochar Optimizer Pro · Claude plugin

Bring [biocharpro.io](https://biocharpro.io) into Claude as an MCP server.
Simulate biochar, compare feedstocks, extract lab-analysis PDFs and score
your project against 5 carbon-removal methodologies — all from any Claude
Code session.

## What it does

Four tools become available to Claude once the plugin is installed:

| Tool | What it does |
|---|---|
| `simulate_biochar` | Runs a pyrolysis simulation for a single feedstock at a given temperature and residence time. Returns carbon content, H/Corg stability ratio, yield, BET surface, pH, and net CO₂e credits per tonne. |
| `compare_feedstocks_batch` | Runs up to 50 feedstock simulations at the same temperature + residence time. Ideal for picking the best biomass for a project. |
| `list_feedstocks` | Lists the 48 built-in feedstocks with full elemental composition. |
| `extract_lab_analysis` | AI-extracts 20+ structured parameters from a biomass/biochar lab-analysis PDF (powered by Gemini 2.5 Flash). |

All tools hit the same REST API at `https://biocharpro.io/api/v1/*`. Usage
counts against the same per-key rate limits (100 req/min Developer,
500 req/min Engineer+).

## Install

### Requirements

- Claude Code installed ([installation docs](https://docs.claude.com/en/docs/claude-code/quickstart))
- A **Developer tier or higher** subscription on biocharpro.io
- An API key — create one at [biocharpro.io/api](https://biocharpro.io/api)

### One-shot (recommended)

Paste into your terminal, replacing `bop_YOUR_API_KEY` with your real key:

```bash
claude mcp add biocharpro \
  --transport http \
  --header "Authorization: Bearer bop_YOUR_API_KEY" \
  https://biocharpro.io/mcp
```

Tools show up in new Claude Code sessions.

### Using the plugin manifest

Alternatively, install this repo as a plugin so you get the `SKILL.md` hints
that help Claude know when to invoke each tool:

```bash
claude plugins add https://github.com/pablokohan30-ux/biocharpro-claude-plugin
```

Then set the `BIOCHARPRO_API_KEY` environment variable (or override the
`Authorization` header in the MCP server config after install).

## Try these prompts

Once connected, open any Claude Code session and try:

> *"Using biocharpro, simulate pine sawdust at 650 °C for 30 minutes. Is it
> certifiable under Puro.earth?"*

> *"Compare pine_sawdust, coffee_husk, rice_straw and corn_stover at 600 °C
> for 30 min. Which gives the best net CO₂e per tonne?"*

> *"List all feedstocks in biocharpro with carbon content above 45%."*

> *"Read this lab-analysis PDF and tell me the H/Corg of the biochar."*

## What you get from biocharpro.io beyond the plugin

The plugin is read-only to the simulation layer. For the full experience —
project management, LCA, multi-methodology scoring, submission packages for
Puro.earth, Isometric, Verra VM0044, EBC and Gold Standard, PDD Builder —
use the web app at [biocharpro.io](https://biocharpro.io).

## Security

- API keys are scoped to your account and can be revoked at any time from
  [biocharpro.io/api](https://biocharpro.io/api).
- The MCP server runs in **stateless mode** — each request is authenticated
  independently and no session data is persisted on our side.
- All traffic over HTTPS. Request/response payloads are not logged beyond
  timestamps + the last-used timestamp per key.
- See [`PRIVACY.md`](./PRIVACY.md) for the full data handling policy.

## Support

- Product: [biocharpro.io](https://biocharpro.io)
- Docs: [biocharpro.io/guide](https://biocharpro.io/guide)
- Issues with the plugin: open an issue in this repo.
- Issues with the API: email the contact listed in the dashboard.

## License

[MIT](./LICENSE).
