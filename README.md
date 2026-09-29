Monthly Airbnb Demand Forecasting — Fort Worth
Time series forecasting project in R (fpp3 / fable ecosystem) that predicts monthly Airbnb demand for Fort Worth from historical review activity. Nine forecasting models are trained and compared on a 6-month holdout; TSLM (linear regression with trend + seasonality) is selected as the production model and used to generate 6-month forecasts with 80% and 95% prediction intervals.
Guided project completed March 2025 under Dr. Jabir Rahman.
Data
data/reviews.csv — 87,961 Airbnb review dates spanning Jan 2012 – Sep 2025, aggregated into monthly demand counts.
The raw file contained reviewer names, IDs, and comment text; it has been stripped down to the date column only (the only column the analysis uses) so no personal information is published.
Methodology
Ingest & aggregate — parse review dates with lubridate, bucket into monthly counts, build a tsibble; apply a log1p transform to stabilize variance.
Exploratory analysis — restrict to post-2022 data for stability; check stationarity with ADF, KPSS, and Phillips-Perron tests plus ndiffs/nsdiffs; decompose with STL to inspect trend, seasonality, and remainder.
Train/test split — hold out the final 6 months for testing.
Model comparison — fit 9 models and rank them by RMSE, MAE, MAPE, and MASE on the holdout, with Ljung-Box tests on residuals to check for leftover autocorrelation.
Production forecast — refit the winning model on the full series, run residual diagnostics (gg_tsresiduals, Ljung-Box), and forecast 6 months ahead with 80%/95% intervals.
Models compared
Model
Specification
Mean
MEAN() baseline
Naïve
NAIVE() baseline
Seasonal Naïve
SNAIVE() baseline
Drift
Random walk with drift
ETS
Automated exponential smoothing
Auto ARIMA
Automated ARIMA selection
Manual SARIMA
ARIMA(pdq(1,0,0))
NNETAR
Neural network autoregression
TSLM ✅
TSLM(log_count ~ trend() + season()) — selected
Full accuracy table and diagnostic plots are in report/airbnb_demand_forecasting.pdf.
Repository structure
time-series-forecasting-r/
├── README.md
├── airbnb_demand_forecasting.Rmd   # full analysis: EDA → 9 models → selection → forecast
├── data/
│   └── reviews.csv                 # review dates only (PII stripped), 1.1 MB
└── report/
    └── airbnb_demand_forecasting.pdf  # knitted report with plots and results
How to run
Requires R with the following packages:
install.packages(c("fpp3", "lubridate", "tseries", "dplyr", "readr", "knitr"))
Then open airbnb_demand_forecasting.Rmd in RStudio and knit — all paths are relative to the repo root, so it runs as-is after cloning.
Skills demonstrated
R · fpp3/fable · tsibble · stationarity testing (ADF/KPSS/PP) · STL decomposition · ARIMA/ETS/NNETAR/TSLM · forecast accuracy metrics (RMSE, MAE, MAPE, MASE) · residual diagnostics (Ljung-Box) · prediction intervals · R Markdown reporting
