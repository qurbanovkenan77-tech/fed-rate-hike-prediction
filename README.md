# Federal Reserve Rate-Hike Prediction Using the U.S. Treasury Yield Curve

## Overview

This project uses the U.S. Treasury yield curve to estimate the probability of Federal Reserve
interest-rate hikes around the remaining scheduled FOMC decision windows in 2026.

The analysis uses four Treasury maturities:

- 3-month Treasury
- 6-month Treasury
- 10-year Treasury
- 30-year Treasury

It also uses yield-curve spreads and gaps between short-term Treasury yields and the Effective
Federal Funds Rate.

## Research Question

Can information in the U.S. Treasury yield curve help estimate the probability of Federal Reserve
monetary tightening around the remaining 2026 FOMC decisions?

## Forecast Date and Decision Windows

The forecast information set is fixed at **September 10, 2026**.

The scheduled decision dates used in the notebook are:

| Month | Scheduled FOMC decision |
|---|---|
| September | September 16, 2026 |
| October | October 28, 2026 |
| November | No regularly scheduled meeting |
| December | December 9, 2026 |

November is reported as **N/A**, rather than 0%, because there is no regularly scheduled FOMC
meeting in that month. Unscheduled intermeeting actions are outside the scope of the model.

## Methodology

The project uses logistic regression with standardized predictors.

Predictors include:

- 3-month Treasury yield
- 6-month Treasury yield
- 10-year Treasury yield
- 30-year Treasury yield
- 6M - 3M spread
- 10Y - 3M spread
- 30Y - 3M spread
- 3M - Effective Federal Funds Rate gap
- 6M - Effective Federal Funds Rate gap

The targets are aligned to the spacing of the remaining 2026 FOMC decision dates:

- September window: forecast date to the September decision
- October window: September decision to the October decision
- December window: October decision to the December decision

This avoids automatically counting an earlier hike again in a later decision window.

The models are trained on observations before 2020 and evaluated on observations from 2020 onward.

## Evaluation

Model performance is evaluated using:

- Accuracy
- ROC-AUC
- Brier score

Accuracy alone can be misleading because rate hikes are relatively rare. ROC-AUC measures the
model's ability to rank higher-risk and lower-risk observations, while the Brier score evaluates
the quality of probability forecasts.

## Data Sources

Data are downloaded from **Federal Reserve Economic Data (FRED)**:

- `DGS3MO` - 3-Month Treasury Constant Maturity Rate
- `DGS6MO` - 6-Month Treasury Constant Maturity Rate
- `DGS10` - 10-Year Treasury Constant Maturity Rate
- `DGS30` - 30-Year Treasury Constant Maturity Rate
- `DFF` - Effective Federal Funds Rate

FOMC meeting dates come from the **Board of Governors of the Federal Reserve System**.

## Project Structure

```text
.
├── fed_rate_hike_prediction_FINAL.ipynb
├── README.md
└── figures/
    ├── treasury_yield_curve.png
    └── fed_hike_probabilities.png
```

The `figures/` directory is created automatically when the notebook is run.

## Generated Figures

The notebook automatically saves two high-resolution PNG files:

1. `figures/treasury_yield_curve.png`
2. `figures/fed_hike_probabilities.png`

The figures are saved at 300 DPI and can be used directly in a report or assignment submission.

## Requirements

Install the required Python packages with:

```bash
pip install pandas numpy matplotlib scikit-learn pandas-datareader
```

## How to Run

1. Open `fed_rate_hike_prediction_FINAL.ipynb`.
2. Select a Python environment containing the required packages.
3. Run all cells from top to bottom.
4. The notebook downloads the FRED data, trains the models, displays the results, and saves the
   two figures into the `figures/` folder.

## Interpretation

The model outputs should be interpreted as **statistical estimates based on Treasury-yield
information**, not as exact market-implied probabilities or guarantees of future Federal Reserve
actions.

The October and December estimates correspond to incremental historical windows aligned to the
spacing of the 2026 meetings. They are not estimates from a full historical meeting-by-meeting
FOMC dataset.

## Reproducibility

The forecast date is intentionally fixed at **September 10, 2026** so that later reruns do not
change the assignment's information set or the forecast horizons.
