# Federal Reserve Rate-Hike Prediction Using a Hazard Rate Model

## Overview

This project uses the U.S. Treasury yield curve and a **discrete-time hazard rate model** to estimate
the probability of the next Federal Reserve interest-rate hike during the remaining scheduled 2026
FOMC decision windows.

Treasury maturities used:

- 3-month
- 6-month
- 10-year
- 30-year

The forecast information set is fixed at **September 10, 2026**.

## Model

The notebook uses a **pooled logistic discrete-time hazard model**.

For decision interval `j`, the hazard is:

`h_j = P(next hike occurs in interval j | no earlier hike has occurred)`

Historical data are converted into person-period form. Each forecasting episode remains in the risk
set until its first observed hike. Period indicators provide the baseline hazard, while Treasury
yields, yield spreads, and short-rate gaps are the explanatory variables.

The notebook also converts the conditional hazards into:

- unconditional probabilities that the first hike occurs in each interval, and
- cumulative probabilities that at least one hike has occurred by each decision date.

## 2026 Decision Windows

| Month | Scheduled decision |
|---|---|
| September | September 16, 2026 |
| October | October 28, 2026 |
| November | No regularly scheduled meeting |
| December | December 9, 2026 |

November is reported as **N/A**, not 0%.

## Data

The project downloads the following FRED series:

- `DGS3MO` — 3-Month Treasury Constant Maturity Rate
- `DGS6MO` — 6-Month Treasury Constant Maturity Rate
- `DGS10` — 10-Year Treasury Constant Maturity Rate
- `DGS30` — 30-Year Treasury Constant Maturity Rate
- `DFF` — Effective Federal Funds Rate

## Evaluation

The hazard model is evaluated using a chronological train/test split and reports:

- Accuracy
- ROC-AUC
- Brier score
- Baseline Brier score

The training and testing episodes are separated so that future outcome windows from the training set
do not overlap the test period.

## Project Files

```text
.
├── fed_rate_hike_prediction_HAZARD_FINAL.ipynb
├── README.md
└── figures/
    ├── treasury_yield_curve.png
    ├── hazard_rate_probabilities.png
    └── cumulative_hike_probability.png
```

The `figures/` folder is created automatically when the notebook is run.

## Requirements

```bash
pip install pandas numpy matplotlib scikit-learn pandas-datareader
```

## How to Run

1. Open `fed_rate_hike_prediction_HAZARD_FINAL.ipynb`.
2. Select the Python environment containing the required packages.
3. Run all cells from top to bottom.
4. Review the final results table under **Estimate the 2026 Hazard Probabilities**.
5. The figures will be saved automatically to the `figures/` folder.

## Interpretation

The **Conditional hazard (%)** column is the main hazard-model result.

The model estimates the probability that the **next** hike occurs in each interval conditional on no
earlier hike. These estimates are statistical forecasts based on Treasury yields; they are not direct
Fed-funds-futures probabilities or guarantees of Federal Reserve action.

## Limitations

The model uses relative historical windows matching the spacing of the remaining 2026 decisions
rather than a complete historical FOMC meeting-level event dataset. It also models the first hike in
an episode rather than repeated hikes.

November 2026 has no regularly scheduled FOMC meeting, so it is shown as N/A.
