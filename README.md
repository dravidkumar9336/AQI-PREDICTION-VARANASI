
# AQI Forecasting — Varanasi (2017–2025)

Time series forecasting project to predict daily **Air Quality Index (AQI)** for Varanasi using historical data from 2017–2025. Three forecasting approaches — **ARIMA**, **SARIMA**, and **Prophet** — are built, evaluated with a rolling one-step-ahead strategy, and compared using MAE / MSE / RMSE / R².

## 📌 Project Overview

- **Goal:** Forecast daily AQI values for Varanasi using classical time series models.
- **Data:** Daily AQI records for Varanasi (2017–2025), sourced as yearly CSV files.
- **Approach:** Data cleaning → EDA → stationarity checks → train/test split (last 90 days held out) → rolling one-step-ahead forecasting for ARIMA, SARIMA, and Prophet → model comparison.

## 🗂️ Repository Structure

```
AQI-Forecasting-Varanasi/
├── data/                              # Yearly AQI CSVs (2017-2025)
├── AQI_Forecasting_Varanasi.ipynb     # Main notebook
├── README.md
└── requirements.txt
```

## 🔍 Workflow

1. **Data Loading & EDA** — Combined yearly CSVs into a single daily series, checked AQI distribution, monthly and yearly trends.
2. **Data Cleaning** — Handled missing values via interpolation within year-month groups, with fallback to group means.
3. **Stationarity Check** — ADF test and ACF/PACF plots on the (differenced) series to guide ARIMA/SARIMA order selection.
4. **Train/Test Split** — Daily log-transformed series (`np.log1p`), last 90 days used as the test set.
5. **Modeling:**
   - **ARIMA** — Order selected via AIC-based grid search; rolling one-step-ahead forecasts on the log series, inverse-transformed with `np.expm1`.
   - **SARIMA** — Weekly seasonality (`s=7`) with AIC-based grid search over seasonal and non-seasonal orders.
   - **Prophet** — Multiplicative seasonality with yearly and weekly components, rolling one-step-ahead forecasts on the original scale.
6. **Evaluation** — MAE, MSE, RMSE, and R² computed for each model on the same 90-day rolling test window.

## 📊 Results

| Model    | MAE    | MSE     | RMSE   | R²    |
|----------|--------|---------|--------|-------|
| ARIMA    | 19.52  | 969.11  | 31.13  | 0.571 |
| SARIMA   | 23.89  | 1172.34 | 34.24  | 0.481 |
| Prophet  | 24.72  | 948.67  | 30.80  | **0.628** |

**Prophet** performed best overall on the original AQI scale, while **ARIMA** on the log-transformed series achieved the strongest fit (R² ≈ 0.92) by stabilizing variance in the right-skewed AQI data (max ~500 vs mean ~140).

### Key takeaways
- Switching from a static long-horizon forecast to a **rolling one-step-ahead** forecast fixed the negative R² seen in an earlier static/monthly approach, by preventing error accumulation.
- **Log transformation** (`np.log1p`) significantly improved ARIMA/SARIMA performance on the skewed AQI series.
- Prophet handled the original (non-log) scale well due to its built-in seasonality and trend components.

## 🛠️ Tech Stack

- Python, Pandas, NumPy
- Statsmodels (ARIMA, SARIMAX, ADF test, ACF/PACF)
- Prophet
- Scikit-learn (evaluation metrics)
- Matplotlib, Seaborn

## ▶️ How to Run

```bash
pip install -r requirements.txt
jupyter notebook AQI_Forecasting_Varanasi.ipynb
```

## 👤 Author

**Dravid Kumar**
M.Sc. Statistics and Computing, Banaras Hindu University (BHU)
