# 🔥 NASA Global Wildfire Insights

A large-scale data science and geospatial analytics project analyzing **global wildfire activity in 2024** using NASA’s FIRMS (Fire Information for Resource Management System) satellite data.  
This project integrates **machine learning, unsupervised learning, and time-series forecasting** to explore, model, and predict wildfire dynamics — providing actionable insights into **when, where, and how fires occur worldwide.**

---

## 📌 Objectives
- Analyze global wildfire patterns using NASA MODIS satellite data.  
- Identify **spatio-temporal trends** and regional fire intensity.  
- Predict fire confidence levels using **classification models**.  
- Explore structure in fire events via **dimensionality reduction** (PCA, t‑SNE) and **K‑Means clustering**.  
- Build a **weekly forecasting pipeline** with ARIMA, Prophet, and XGBoost to anticipate future fire counts.

---

## ⚙️ Methodology
### 🔹 Data Analysis & Modeling
- Preprocessed >100K satellite records (cleaning, encoding, outlier removal).  
- Trained a **RandomForestClassifier** to predict satellite‑reported fire confidence.  
- Applied **PCA/t‑SNE** for separability visualization and latent‑space exploration.  
- Implemented **K‑Means** to find clusters within fire occurrences.  
- Evaluated using **Precision, Recall, F1‑Score**, and **Adjusted Rand Index (ARI)**.

### 🔹 Forecasting Layer
- Aggregated fire detections into **weekly global grids (0.25°)**.  
- Engineered lag features (`lag1`, `lag4_sum`) and seasonal encodings (`w_sin`, `w_cos`).  
- Compared multiple forecasting approaches:  
  - Baselines: *Naïve, Moving Average (MA(4))*  
  - **ARIMA(2,1,2)** (diagnostic plots: ACF, Ljung‑Box)  
  - **Prophet** (weekly seasonality)  
  - **XGBoost** with bias correction & reconciliation  
- Evaluated with **MAE**, **RMSE**, and **SMAPE** metrics.

---

## 📊 Results Summary
| Model | Category | Performance |
|--------|-----------|-------------|
| Random Forest | Classification | F1 ≈ 0.77 |
| ARIMA | Forecasting | Robust global baseline |
| Prophet | Forecasting | Under‑forecasts late‑season peaks |
| XGBoost | Forecasting | Lowest RMSE at per‑cell level |

**Best Overall:** *XGBoost + ARIMA reconciliation* → combines spatial detail and global accuracy.

---

## 🌍 Insights
- **Seasonal cycles** strongly influence wildfire occurrence.  
- **Brightness & FRP** are key predictive features for confidence.  
- Clustering shows overlapping but interpretable spatial groupings.  
- **Forecasting models** capture short‑term variations effectively.  

---

## 🚀 Quick Start
```bash
git clone https://github.com/ziaee-mohammad/NASA-Global-Wildfire-Insights.git
cd NASA-Global-Wildfire-Insights
pip install -r requirements.txt
jupyter notebook notebooks/NASA_Wildfire_Analysis.ipynb
```

---

## 🧠 Tech Stack
- Python • Pandas • NumPy  
- Scikit‑learn • XGBoost • Prophet • Statsmodels  
- Matplotlib • Seaborn • Plotly  
- GeoPandas • Folium

---

## 🧾 Author
**Mohammad Ziaee** — Data Science & AI Enthusiast  
GitHub: [https://github.com/ziaee-mohammad](https://github.com/ziaee-mohammad)

---

## 🏷️ Tags
`NASA` • `Wildfire` • `Geospatial` • `Time-Series` • `Forecasting` • `Climate` • `Earth Observation` • `Machine Learning` • `Data Science`
