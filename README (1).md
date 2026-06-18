# Palm Oil Price Forecasting — ARIMA vs LSTM

This repository contains a single Google Colab notebook, **`Palm_Oil_Price_Forecasting_ARIMA_LSTM.ipynb`**, that forecasts monthly Malaysian crude palm oil prices using two approaches — a classical **ARIMA** model and a deep-learning **LSTM** model — and compares their accuracy. It is adapted from the thesis *"Peramalan Harga Minyak Sawit di Malaysia Menggunakan Model ARIMA dan LSTM"* (Saranya A/P Ramis, UKM), translated and reorganised into an English, report-style notebook with explanatory markdown alongside runnable code.

## Contents

- `Palm_Oil_Price_Forecasting_ARIMA_LSTM.ipynb` — the full notebook (markdown explanations + code).

## Requirements

The notebook is designed to run on **Google Colab** (free tier is sufficient). It installs/uses:

- `pandas`, `numpy`, `matplotlib`, `seaborn`
- `pmdarima` (installed automatically via `!pip install pmdarima -q`)
- `statsmodels` (ADF test, ACF/PACF plots)
- `scikit-learn` (MinMaxScaler, RMSE/MAPE metrics)
- `tensorflow` / `keras` (LSTM model — pre-installed on Colab)

If running locally instead of Colab, remove the `google.colab` import/`drive.mount()` cell and point `file_path` to a local CSV.

## Data Format

The notebook expects a CSV file with at least two columns:

| Column | Description |
|---|---|
| `Date` | Date of the observation (default parsed as `MM/DD/YYYY`; adjust the `format=` argument if yours differs) |
| `Price` | Closing price; may contain comma thousands separators (e.g. `4,250.50`) — these are stripped automatically |

This matches the format of monthly palm oil price data exported from Investing.com.

## How to Run

1. Upload your CSV (e.g. `palm_oil.csv`) to Google Drive, e.g. under `MyDrive/Data Science/palm_oil.csv`.
2. Open the notebook in Google Colab.
3. Run the **Setup** section — this mounts your Drive when prompted.
4. Update the `file_path` variable in **both** places it appears (once before the ARIMA section, once before the LSTM section) to match your file's location.
5. Run all cells from top to bottom (`Runtime → Run all`). The `auto_arima` search and the rolling-forecast loops (for both ARIMA and LSTM) are the slowest steps — expect these to take longer than the rest of the notebook combined, especially on long test sets, since a model is re-fitted at every step.

## Notebook Structure

1. **Introduction & Methodology** — background on palm oil price volatility and an overview of the ARIMA and LSTM approaches, including the ARIMA equation and RMSE/MAPE formulas.
2. **Setup & Data Loading** — installs/imports, Drive mount, data cleaning, 80/20 train-test split.
3. **Exploratory Data Analysis** — full price history plot and train-test split visualisation.
4. **ARIMA Modelling** — ADF stationarity test, ACF/PACF plots, differencing, `auto_arima` model selection, static forecast, and rolling (walk-forward) forecast, each with RMSE/MAPE and a comparison plot.
5. **LSTM Modelling** — MinMaxScaler normalisation, sequence windowing (12-month lookback), a Bidirectional LSTM → Dropout → LSTM → Dense architecture, training (20 epochs, batch size 16), static forecast, and rolling forecast, each with RMSE/MAPE and a comparison plot.
6. **Model Comparison** — combined plot of both models' rolling forecasts against actual prices, plus the thesis's benchmark accuracy table.
7. **Conclusion & References** — summary of findings and the original thesis's reference list.

## Reference Benchmark Results (from the original thesis)

| Model | RMSE | MAPE |
|---|---|---|
| ARIMA(0,1,0) | 768.27 | 10.81% |
| LSTM | 662.32 | 8.95% |

Your own results will differ depending on your dataset's date range, length, and any random initialisation in the LSTM training — re-run the notebook on your data to generate your own figures for comparison.

## Notes / Fixes Applied

While translating the original script into this notebook, a few issues were corrected:

- Removed a duplicate `adfuller()` call.
- Removed a dead/buggy code block in the LSTM rolling-forecast section where indentation caused predictions to be appended outside the loop.
- Fixed a typo (`predicted_pricess` → `predicted_price`).
- Made the historical price chart title dynamically reflect the actual date range of the loaded data, rather than a hardcoded date range.
- Standardised all metric calculations to consistently reference the `Price` column.

## Citation

If you use or build on this work, please cite the original thesis:

> Ramis, S. 2024. *Peramalan Harga Minyak Sawit di Malaysia Menggunakan Model ARIMA dan LSTM*. Universiti Kebangsaan Malaysia.
