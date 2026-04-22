---
name: list-feedstocks
description: Use when the user wants to see what feedstocks are available on biocharpro, or when you need to discover valid feedstock IDs before calling `simulate_biochar` or `compare_feedstocks_batch`. Invokes `list_feedstocks` on the biocharpro MCP server.
---

# List feedstocks

Returns all 48 feedstocks in the biocharpro database with their elemental
composition (C, H, N, S, O, ash, moisture). Use this to:

1. Discover available IDs before calling another tool.
2. Answer user questions like "what feedstocks do you support?" or
   "do you have peanut shells?"
3. Filter by composition: "list feedstocks with > 45% carbon content" or
   "show me the driest feedstocks" (low moisture).

## No inputs required

Call with an empty object: `{}`.

## Output

`{ feedstocks: [ { id, name, carbon, hydrogen, nitrogen, sulfur, oxygen, ash,
moisture, source } ], count }`. 48 entries in total as of this release.

## Categories represented

- **Agricultural residues**: rice straw/husk, corn stover, wheat straw,
  sugarcane bagasse, cotton stalks, sunflower, soybean, coffee husk, etc.
- **Forestry**: pine sawdust, eucalyptus, hardwood, softwood, bark chips.
- **Energy crops**: miscanthus, switchgrass, poplar.
- **Food processing residues**: olive pits, grape marc, nutshells.
- **Manure / organic waste**: poultry, dairy manure.
- **Aquatic / other**: algae, water hyacinth, digestate.

If the user names a biomass you can't find in the list, recommend they upload
a lab-analysis PDF via `extract_lab_analysis` and use a custom feedstock in
`simulate_biochar`.
