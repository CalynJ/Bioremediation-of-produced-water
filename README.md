# Produced Water: Volume Decline & PFAS Persistence

R analysis of produced water from three oil and gas wells in the Niobrara formation, Denver Basin, combining production volume decline modeling with PFAS (per- and polyfluoroalkyl substances) concentration data to examine whether treatment burden actually declines as fast as volume does.

## Key finding

Water volume from these wells declines sharply and predictably in the first few months of production, dropping roughly 80-90% from peak within the first 60-90 days. Total PFAS concentration does not follow the same pattern. Across a full year of production, PFAS levels stayed in a comparable range from day 1 through day 367, in some wells even ticking slightly upward late in the year rather than declining alongside volume.

In practical terms: less water is being produced over time, but the water that remains isn't necessarily getting "cleaner." Treatment planning based on volume alone risks underestimating the PFAS load still present in a well's later-life production.

## Data sources

- **Production volume**: `PWPFAS_TS_PRODUCTIONDATA.csv` — monthly water production (barrels) per well, October 2018 through September 2019
- **PFAS results**: `PWPFAS_RESULTS.csv` — targeted PFAS panel (EPA Draft 1633 method) and total oxidizable precursor (TOP) results per well, sampled at five points across each well's first year of production. Sourced from the U.S. Geological Survey data release: Varonka, M.S., Jubb, A.M., McDevitt, B., Shelton, J.L., Barnhart, E.P., Akob, D.M., and Cozzarelli, I.M., 2025, *Per- and polyfluoroalkyl substances (PFAS) in produced water from the Denver Basin*, USGS data release, https://doi.org/10.5066/P13SQVJA

## A note on joining the two datasets

The two files don't share a usable calendar date field. The PFAS file's `ANALYSIS_DATE` reflects when the lab ran the test, not when the water was sampled, so it doesn't correspond to the production dates in the volume file. Instead, both datasets are aligned using **days since production start** (`PROD_DAYS` in the PFAS data, calculated from the production date in the volume data), which is the actual shared timeline both datasets are really tracking.

## What the analysis does

1. **Decline curve modeling**: fits an exponential decay model (`WATER_BBL = a * exp(-b * t)`) to each well's monthly volume, with a 6-month forward forecast
2. **PFAS totals**: sums detected PFAS compounds per sample (targeted panel only; TOP/precursor results are kept separate since they measure a different thing) — "ND" (not detected) results are treated as zero
3. **Combined visualization**: a dual-axis plot showing volume decline against total PFAS concentration on the same production-day timeline, to make the divergence between the two visually obvious

## Files

- `produced_water_analysis.R` — full analysis script
- `PWPFAS_TS_PRODUCTIONDATA.csv` — production volume data
- `PWPFAS_RESULTS.csv` — PFAS concentration results

## Running it

Requires R with `tidyverse`, `lubridate`, and `scales`:

```r
install.packages(c("tidyverse", "lubridate", "scales"))
```

Run `produced_water_analysis.R` from a folder containing both CSV files.

## Caveats

- PFAS sampling is sparse relative to volume data (5 points per well vs. 12 monthly readings), so the PFAS trend is directional rather than finely resolved
- Lab result flags (e.g., estimated/qualified detections) are present in the raw PFAS data and worth reviewing before treating any single value as precise
- This is not water quality or regulatory guidance — it's an exploratory look at how two real, related datasets tell a more complete story together than either does alone
