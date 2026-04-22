---
name: simulate-biochar
description: Use when the user asks about biochar properties (carbon content, yield, H/Corg ratio, BET surface, pH, CO₂e credits) for a given feedstock under specific pyrolysis conditions. Invokes `simulate_biochar` on the biocharpro MCP server.
---

# Simulate biochar

Single-feedstock pyrolysis simulation against a peer-reviewed empirical model
calibrated on CINDECA/CONICET data and major biochar literature. Returns the
biochar composition you would get from a given biomass at a given temperature
and residence time.

## When to use

Use this skill when the user asks any of:

- "What's the H/Corg ratio of pine sawdust at 650 °C?"
- "Simulate coffee husk biochar at 550 °C for 30 minutes."
- "How much CO₂e do I get per tonne of rice straw biochar?"
- "What yield can I expect from wheat straw at 700 °C?"
- "Is my biochar stable enough for Puro.earth certification?" (check `hCorgRatio < 0.7`)
- "Is it stable enough for 1000-year durability?" (check `hCorgRatio < 0.4`)

You should also use it when the user hasn't been explicit but is clearly
scoping a biochar project and needs a number (yield, C content, CO₂e).

## Required inputs

- **Feedstock**: either a `feedstockId` from the built-in 48-feedstock database
  (pine_sawdust, coffee_husk, rice_straw, corn_stover, sugarcane_bagasse, etc.)
  OR a `customFeedstock` object with `{ name, C, H, O }` and optional
  `{ N, S, ash, moisture }` (all on % dry basis).
- **Temperature**: 300–900 °C.
- **Residence time**: 5–180 min.

Call `list_feedstocks` first if the user names a biomass you're not sure
maps to a feedstockId.

## Output interpretation

Key fields to summarise for the user:

- `carbonContent` (%): typical 50–85%. ≥10% minimum to certify (Puro.earth),
  ≥50% for Verra VM0044 and Isometric.
- `hCorgRatio`: **the most important number**. <0.7 = certifiable as stable
  biochar (BC-2 class, 100+ years). <0.4 = premium (BC-1 class, 1000+ years;
  required for Isometric 1000-yr tier and Verra high-permanence class).
- `yield` (%): 25–40% typical.
- `betSurface` (m²/g): 100–500 typical; high = good water/nutrient retention.
- `pH`: 7–11 typical; biochar is usually basic.
- `grossCo2ePerTonne` / `netCo2ePerTonne`: tCO₂e per tonne of biochar (gross
  before operational deductions, net after LCA).
- `ebcClass`: EBC stability tier (BC-1, BC-2, etc.)

## Temperature rule of thumb

- <500 °C: H/Corg usually too high for certification.
- 550–650 °C: **sweet spot** for most biomass — balance of yield + stability.
- \>700 °C: maximises fixed carbon + lowest H/Corg, but yield drops sharply.

If the user is asking about certification, recommend 550–650 °C as the
operational minimum.
