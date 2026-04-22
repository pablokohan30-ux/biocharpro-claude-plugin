---
name: extract-lab-analysis
description: Use when the user has a lab-analysis PDF of biomass or biochar and wants the numbers parsed into structured JSON (elemental composition, heavy metals, pyrolysis parameters). Invokes `extract_lab_analysis` on the biocharpro MCP server. Powered by Gemini 2.5 Flash.
---

# Extract lab analysis PDF

Send a base64-encoded biomass or biochar lab-analysis PDF and get back 20+
structured parameters. Useful when the user is trying to onboard a feedstock
that isn't in the built-in database.

## When to use

- "I have a lab report from Eurofins / SGS / a local lab — can you parse it?"
- "Read this PDF and tell me the H/Corg."
- "Extract the heavy metals from this analysis."
- "I don't see my biomass in your feedstock list." (recommend this as
  follow-up: upload their lab PDF, get a custom feedstock object, then
  pass it to `simulate_biochar`.)

## Required inputs

- **pdfBase64**: base64-encoded PDF content. Max 10 MB decoded. NO data URL
  prefix (the bare base64 string).
- **pdfName**: optional filename, useful for traceability in logs.

If the user gives you the file path instead of base64, ask them to encode it
first (or if the runtime allows, read the file and base64-encode it before
calling the tool).

## Output

Returns `extracted` containing (nulls allowed per field):

- **biomassName**: common or scientific name.
- **biomass**: proximate + elemental (C, H, N, S, O, ash, moisture,
  volatileMatter, fixedCarbon) as % dry basis.
- **biochar**: C, H, N, S, O, HCorgMolar, BET (m²/g), poreVolume, poreDiameter,
  pH, thermalStability.
- **pyrolysis**: temperature (°C), residenceTime (min), atmosphere,
  heatingRate.
- **heavyMetals**: Pb, Cd, Cr, Cu, Ni, Zn, Hg, As in µg/g (ppm).
- **source**: document citation if present.
- **notes**: caveats the lab wrote.

## Follow-up actions

After extraction, typical next steps:

1. If the user wants to simulate their biomass, feed the `biomass` fields
   into `simulate_biochar`'s `customFeedstock` parameter.
2. If they want to verify H/Corg against certification thresholds, compare
   `biochar.HCorgMolar` against 0.7 (Puro.earth / EBC / Verra VM0044) and
   0.4 (Isometric 1000-yr tier).
3. If they want Puro.earth / Verra certification upstream, their `heavyMetals`
   must be within EBC limits — mention the EBC certification requirement.

## Error codes

- `PDF_TOO_LARGE`: > 10 MB. Ask them to trim or re-scan at lower DPI.
- `AI_QUOTA_EXCEEDED` / `AI_UNAVAILABLE`: transient; retry in a few minutes.
- `EXTRACTION_FAILED`: PDF unreadable (blurry scan, handwritten, etc.).
  Suggest providing a clearer copy or typing the key values manually.
