# PADD 3 Sour & Heavy Crude — Supply/Demand Balance Model

A month-by-month supply and demand balance for medium/heavy **sour** and **heavy** crude on the **US Gulf Coast (PADD 3)**, built as a fully native-Excel decision tool. It answers one question: *is the Gulf Coast short or long on sour/heavy crude this month?*

## What it does

Adds up every barrel of sour/heavy crude that shows up in PADD 3 — GoM production, waterborne imports, pipeline barrels from Cushing, and SPR releases — subtracts what leaves (exports), and compares that to how much the major Gulf Coast refineries want to run. The difference is the balance:

- **Negative** → region is short; it must bid barrels away from others (differentials firm)
- **Positive** → barrels are looking for a home (differentials soften)

## Key idea: demand is coker-anchored

A refinery's appetite for heavy crude is set by its **coker**, not its overall size. The coker digests the heavy bottom of the barrel, so heavy demand is anchored to coking capacity:

```
Heavy run (max) = MIN( CDU × utilization × 95%,  Coker × 95% ÷ 38% )
Heavy demand    = midpoint of HIGH (coker max) and LOW (HIGH × low-factor)
Sour demand     = Heavy + 45% × (Crude runs − Heavy)
Balance         = Net supply − Demand
```

## Workbook layout

The workbook is grouped into color-coded sections:

| Section | Tabs |
| --- | --- |
| 🔵 **DATA** (shared inputs) | Imports & Exports · Pipelines · Refiner Demand |
| 🟦 **SOUR** | Supply · Balance |
| 🟥 **HEAVY** | Refiners · Balance |
| 🟩 **Reference** | Seasonal Normal (10-yr) |

100% native Excel — live formulas, no macros, no dependencies. Color code: **blue = input you can change**, **black = formula**, **yellow = assumption to challenge**.

## Repo contents

| File | Purpose |
| --- | --- |
| `PADD3_Sour_Supply_Demand_2026.xlsx` | The model (the deliverable) |
| `build_model.py` | Rebuilds the workbook from scratch (authoring tool only) |
| `data_imports.xlsx` / `data_exports.xlsx` | Trade volumes by grade/month — refresh these monthly |
| `eia_*.xls` | Raw EIA history for the seasonal tab |
| `PADD3_Model_Handoff_Guide.docx` | Plain-English walkthrough for new users |
| `PADD3_Research_Sources.xlsx` | All data sources with clickable links |

## Data sources

EIA (production, imports, pipeline receipts, refinery capacity), DOE/SPR (spr.doe.gov), pipeline operators (Enbridge, Genesis, South Bow), and a market-data platform for grade-level trade flows. See `PADD3_Research_Sources.xlsx` for the full list with series IDs and links.

> Tip: any EIA series downloads as a spreadsheet via `eia.gov/dnav/pet/hist_xls/<SERIES_ID>m.xls` (`w.xls` for weekly).

## Updating it

1. Refresh `data_imports.xlsx` / `data_exports.xlsx` from the platform
2. Update GoM production, refinery utilization, and pipeline receipts when EIA's monthly data publishes
3. Update the SPR row from DOE / EIA weekly stocks
4. Re-run `python build_model.py` (or edit the workbook directly)

## Caveats

- Import/export files cover tracked terminals (~half of total PADD 3 imports) and are used unscaled — watch the **trend**, not the absolute level
- Demand is capacity-driven (modeled), not measured — hence the LOW/HIGH range
- GoM grade splits and line-by-line pipeline flows are estimates reconciled to EIA totals
