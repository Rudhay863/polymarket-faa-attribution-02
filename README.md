# Polymarket FAA Outage Attribution Forecaster

Entry for [polymarket-faa-attribution-02](https://make.fun/bounty/polymarket-faa-attribution-02) ($250): forecast each FAA outage attribution market's Yes price minute by minute, 4 hours ahead, as five quantiles (5%, 25%, 50%, 75%, 95%).

## Model (`main.py`, Python 3.13 standard library only, deterministic)

- **Median:** the last price (persistence). Prediction-market prices are close to a martingale over four hours; trend and pullback variants scored worse on the practice cases.
- **Spread:** per-minute volatility from the winsorised RMS of one-minute price changes — geometric mean of the 6-hour and 24-hour windows, replaced by the 1-hour RMS when the last hour was jumpier; clamped to [0.001, 0.02].
- **Horizon:** sigma(h) = sigma1 * h^0.25 — strongly sublinear, because these prices are sticky and mostly mean-reverting over hours.
- **Quantile offsets:** z = [-1.8, -0.67449, 0, +0.67449, +2.0] — wider tails than the normal, widest on the upside.

## Practice score (local evaluator, answers from the next case's file, exact)

Mean pinball loss **0.307342** on cases p00–p04 (p00 0.457, p01 0.313, p02 0.337, p03 0.262, p04 0.204) vs the house baseline's 0.392518 on the same cases (**-21.7%**). The house baseline scores 0.363589 on the full 6-case practice set.

## Run

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py
```

## Referral

Try make.fun yourself: https://make.fun/r/xk7vph3j
