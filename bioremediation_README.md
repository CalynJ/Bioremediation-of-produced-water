# Produced Water Volume Forecasting

R analysis of produced water volumes across three oil and gas wells, using decline curve modeling to forecast future treatment loads for bioremediation planning.

## Background

Produced water is a byproduct of oil and gas extraction that requires treatment before disposal or reuse. Understanding how production volume declines over a well's life helps treatment operations plan capacity and resources ahead of time, rather than reacting to volume after the fact.

## What this does

- **Time series analysis** of monthly water production (barrels) across three wells (NWTS-1, NWTS-2, NWTS-3) over a 12-month window
- **Decline curve modeling**: fits an exponential decay model (`WATER_BBL = a * exp(-b * t)`) to each well's production history using non-linear least squares (`nls()`)
- **6-month forecasting**: projects each well's future production volume based on its fitted decay curve, giving a forward-looking estimate of treatment load

## Files

- `produced_water_analysis.R` — full analysis script (data loading, EDA, visualization, decay modeling, forecasting)
- `PWPFAS_TS_PRODUCTIONDATA.csv` — monthly production volume by well, Oct 2018–Sep 2019

## Verified results

Decay model fit to each well (verified against the actual data):

| Well | Decay rate half-life | Pattern |
|------|----------------------|---------|
| NWTS-1 | ~1.7 months | Fastest decline |
| NWTS-2 | ~3.3 months | Slowest decline, most stable |
| NWTS-3 | ~2.4 months | Moderate decline |

A shorter half-life means that well's production drops off faster — useful for anticipating which wells will need less treatment capacity sooner.

## Running it

Requires R with the `tidyverse`, `lubridate`, and `scales` packages installed:

```r
install.packages(c("tidyverse", "lubridate", "scales"))
```

Then run `produced_water_analysis.R` from a folder containing the CSV.
