---
name: compare-feedstocks
description: Use when the user wants to compare multiple biochar feedstocks side-by-side at the same pyrolysis temperature and residence time. Invokes `compare_feedstocks_batch` on the biocharpro MCP server. Much faster than calling `simulate_biochar` one by one.
---

# Compare feedstocks (batch)

Run up to 50 feedstock simulations in one request, all at the SAME temperature
and residence time. Ideal for deciding which biomass to invest in before
building a plant.

## When to use

- "Which gives me more carbon: pine sawdust or coffee husk at 600 °C?"
- "Compare the top 5 agricultural residues at 650 °C for 30 min."
- "I have access to rice straw, corn stover and wheat straw. Which gives the
  best H/Corg?"
- "Show me yield vs carbon trade-off for 10 biomasses at 700 °C."

If the user asks about a single feedstock, use `simulate_biochar` instead.

## Required inputs

- **feedstockIds**: array of 1–50 string IDs from the feedstock database.
  Call `list_feedstocks` first to get valid IDs.
- **temperature**: 300–900 °C, same for all feedstocks.
- **residenceTime**: 5–180 min, same for all feedstocks.

## Output interpretation

Returns `results`: one row per feedstock with `feedstockId`, `name`,
`carbonContent`, `yield`, `hCorgRatio`, `netCo2ePerTonne`, `ebcClass`,
`betSurface`, `pH`. Sort in your response by whichever metric the user cares
about — usually `netCo2ePerTonne` for CDR projects, `hCorgRatio` for
durability-focused ones, or `carbonContent` for tonnage-focused ones.

If any row has `error: "Unknown feedstock"`, drop it from the final table
and mention which IDs were skipped.

## Framing the comparison

- Present as a table (Markdown) with feedstocks as rows. Sort by the metric
  the user asked about.
- Highlight the winner and why (e.g. "pine sawdust wins on net CO₂e, but
  coffee husk wins on yield — pick based on whether you're paid per credit
  or per tonne of biochar").
- Call out if any feedstock crosses a certification threshold (H/Corg < 0.7
  = certifiable, < 0.4 = 1000-year tier).
