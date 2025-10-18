# 🔥 NASA Global Wildfire Insights

This project presents a comprehensive data science analysis of global wildfire activity during 2024 using satellite data from NASA’s FIRMS (Fire Information for Resource Management System). It combines **geospatial exploration**, **classification modeling**, **unsupervised learning**, **and a weekly spatio-temporal forecasting layer** to understand when, where, and how fires occur — and to forecast short-term patterns.

---

## 📌 Objectives

- Analyze global fire detections in 2024 using MODIS satellite data.
- Uncover spatio-temporal patterns in wildfire activity.
- Assess fire intensity and confidence levels.
- Build classification models to predict fire confidence.
- Apply dimensionality reduction (PCA, t-SNE) to explore class separability.
- Use KMeans clustering to identify latent groupings in fire events.
- **NEW:** Deliver a simple, reproducible **weekly forecasting** baseline with transparent evaluation and exportable artifacts.

---

## 📊 Methodology

### 1) Analysis & Modeling (existing)
- **Data Preprocessing:** Cleaned, encoded, and sampled over 100,000 fire records.
- **Supervised Learning:** Trained a `RandomForestClassifier` to predict fire confidence class.
- **Dimensionality Reduction:** Applied `PCA` and `t-SNE` to visualize separability.
- **Clustering:** Used `KMeans` in embedded spaces to explore natural groupings.
- **Evaluation:** Assessed models via precision/recall/F1-score and Adjusted Rand Index.

### ✅ Key Insights (analysis)
- Some satellite features show clear correlation with fire confidence.
- The Random Forest model achieved meaningful classification performance.
- PCA/t-SNE revealed overlap between fire classes, highlighting complexity in the feature space.
- Clustering alignment with true classes was limited — unsupervised learning alone doesn’t capture confidence well without supervision.

---

## 🔮 Weekly Forecasting Layer (new)

We added a **weekly spatio-temporal forecasting** pipeline on a simple 0.25° lat/lon grid.

### Data & Features
- Aggregate FIRMS points → **weekly counts per 0.25° bin** (anchored to Mondays)
- Per-cell features:
  - `lag1` (last week)
  - `lag4_sum` (sum of prior 4 weeks)
  - weekly seasonality via `w_sin`, `w_cos`

### Train / Validation / Test
- **Time-aware split:** first **80%** of weeks = **train**, last **20%** = **test**
- Within train, the last **10%** of weeks = **validation** for tuning/bias checks

### Models Compared
- **Baselines:** `Naïve` (copy last week), `MA(4)` (4-week moving average)
- **ARIMA(2,1,2)** on the **global** weekly series (diagnostics: Q–Q, ACF, Ljung–Box)
- **Prophet** with weekly seasonality
- **XGBoost** on per-cell features; predictions summed to global for comparison
- **Bias correction:** add mean validation residual to de-bias forecasts
- **Log-target:** train on `log1p(count)` and back-transform to calm peaks
- **(Optional) Reconciliation:** keep XGB spatial detail but **scale weekly totals** to match ARIMA’s global forecast

### Evaluation & Outputs
- Metrics: **MAE**, **RMSE**, **SMAPE** (per-cell and global sums)
- Plots: global test curve vs. forecasts; XGBoost feature importance
- Artifacts (written under `data/processed/`):
  - `firms_weekly.parquet`
  - `metrics.csv` (per-model scores)
  - `forecast_*.parquet` (global curves per model)
  - *(optional)* reconciled per-cell/global forecasts

> **Result snapshot (typical run):** ARIMA is a strong, stable **global** baseline; Prophet tends to under-forecast late season; **XGBoost + simple bias correction** performs best on RMSE at the per-cell level, and **reconciling XGB to ARIMA totals** yields globally consistent forecasts while keeping spatial detail. See the notebook’s final tables/plots and `metrics.csv` for exact numbers.
