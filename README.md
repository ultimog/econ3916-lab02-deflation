# econ3916-lab02-deflation
# Deflating Economic Data — Nominal vs. Real

## Objective

An applied treatment of price-index deflation, converting nominal wage and commodity-price series into constant dollars to separate changes in the general price level from changes in real purchasing power.

## Data

Two Federal Reserve Economic Data series — the Consumer Price Index for All Urban Consumers (`CPIAUCSL`, 955 monthly observations, January 1947 to August 2026) and Average Hourly Earnings of Production and Nonsupervisory Employees (`AHETPI`, monthly from January 1964) — together with the United States component of The Economist's Big Mac Index, 45 semi-annual observations from April 2000 to July 2026.

## Methodology

- Retrieved both FRED series through the publisher's public CSV endpoint, which requires no credential. Column names are assigned positionally rather than trusted from the response header, and non-numeric placeholders are coerced to nulls, so that an upstream change in FRED's formatting does not silently corrupt the parse.
- Applied `deflate_series()`, which rescales a nominal series by the ratio of base-year CPI to contemporaneous CPI, expressing every observation in a single year's dollars. The base-year CPI is the mean of that year's monthly observations rather than a single month, avoiding sensitivity to within-year variation.
- Reconciled series of differing frequency. The Big Mac index is published semi-annually on dates that do not consistently coincide with CPI observation dates, so each price was matched to the most recent CPI reading at or before it. A direct index join would have discarded the non-matching observations without warning.
- Validated the deflation against three conditions: that real and nominal values coincide in the base year, that the base-year CPI is a within-year average rather than a single month, and that the three growth rates satisfy the multiplicative identity (1 + nominal) = (1 + real) × (1 + CPI).
- Built an interactive explorer with a base-year control, demonstrating that the choice of base year rescales the level of a real series by a constant while leaving its growth rate unchanged. Its output was verified against the independently checked Step 8 figures.

## Key Findings

**Nominal series are not comparable across time, and the divergence is large.** Nominal average hourly earnings rose from $2.50 to $32.53 over the sample, a thirteen-fold increase. Deflated to constant 2020 dollars, the same series moves from $20.92 to $25.20 — a real gain of roughly twenty percent across six decades. Any narrative constructed from the nominal series alone is measuring the denominator rather than the quantity of interest.

**Real earnings peaked in the early 1970s and did not durably recover.** The constant-dollar series reaches a local maximum near $24.50 around 1973, coincident with the collapse of the Bretton Woods exchange rate regime, the first OPEC supply shock, and the divergence of wage growth from productivity growth. Real earnings declined through the mid-1990s, bottoming near $19.80, before resuming a slow recovery.

**The 2020 discontinuity in real earnings is consistent with a composition effect rather than a wage effect.** The series exhibits a near-vertical increase in early 2020 that is inconsistent with the gradual movement characteristic of the rest of the sample. Average hourly earnings is a mean across the employed; the concentrated loss of low-wage service employment during the pandemic would raise that mean without any individual receiving an increase. A smaller instance of the same pattern is visible in 2009.

**Most of the Big Mac's price increase is monetary, not real.** The US price rose from $2.24 to $6.22 between April 2000 and July 2026, a nominal increase of 178%. In constant 2020 dollars the same series moves from $3.39 to $4.84, a real increase of 43%, against a CPI increase of 95% over identical dates. The real series has been approximately flat since 2014, indicating that recent price-tag growth reflects the declining value of the dollar rather than a change in the good's relative price.

**Growth rates compound and cannot be differenced.** The three measured rates satisfy (1 + 178%) = (1 + 43%) × (1 + 95%). Subtracting the real rate from the nominal rate yields 135 percentage points, which does not recover the CPI's 95% — the discrepancy being inflation applied to the real growth itself. Over short horizons the approximation is tolerable; over twenty-six years it is not.

## Limitations

`AHETPI` covers production and nonsupervisory employees and reports a mean rather than a median, so it is sensitive to compositional change in the employed population, as the 2020 discontinuity illustrates. CPI deflation measures a series against a fixed consumption basket that no individual household faces exactly, and a single good such as a Big Mac may move relative to that basket for reasons unrelated to monetary conditions. The base-year convention affects reported dollar levels but not growth rates, so levels should not be compared across analyses using different base years. The Big Mac series is sampled twice yearly; charts of it are plotted with markers so that the observation frequency remains visible rather than implied by interpolation.
