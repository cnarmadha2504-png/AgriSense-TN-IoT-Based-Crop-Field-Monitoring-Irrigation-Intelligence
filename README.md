# 🌾 AgriSense TN — IoT-Based Crop Field Monitoring & Irrigation Intelligence

> **Simulated IoT sensor data analysis for Tamil Nadu agriculture using Python, Machine Learning, LIME explainability, and Power BI dashboard**

---

## 📌 Project Overview

AgriSense TN is an end-to-end IoT data analysis project that simulates real-time crop field sensor monitoring across three major agricultural districts of Tamil Nadu. The project predicts irrigation needs, detects anomalies, and explains model decisions using LIME — mimicking real-world smart farming systems.

---

## 🗺️ District & Crop Mapping

| District | Crop | Reason |
|---|---|---|
| Thanjavur | Rice | Tamil Nadu's rice bowl — Cauvery delta region |
| Erode | Turmeric | Largest turmeric market in Asia |
| Coimbatore | Tomato | Major vegetable farming belt |

---
## 📊 Dataset Description

**Type:** Simulated IoT time-series sensor data
**Period:** June – November 2024
**Frequency:** Hourly readings
**Total Records:** 13,107 rows × 11 columns

### Why Simulated?

Real-time IoT sensor data from Tamil Nadu agricultural fields is not publicly available. This dataset was synthetically generated based on:
- **IMD (India Meteorological Department)** seasonal climate patterns for Tamil Nadu
- **ICAR agronomic guidelines** for crop-specific soil requirements
- **Realistic sensor noise** and anomaly injection to mimic real field conditions

This is standard practice in IoT research and smart agriculture projects where live sensor infrastructure is unavailable.

### Dataset Columns

| Column | Unit | Description |
|---|---|---|
| timestamp | datetime | Hourly reading timestamp |
| district | categorical | Thanjavur / Erode / Coimbatore |
| crop | categorical | Rice / Turmeric / Tomato |
| soil_moisture | % | Volumetric water content in soil |
| soil_temp_c | °C | Soil temperature at 10cm depth |
| air_temp_c | °C | Ambient air temperature |
| humidity_pct | % | Relative humidity |
| rainfall_mm | mm | Hourly rainfall measurement |
| nitrogen_mg_kg | mg/kg | Soil nitrogen concentration |
| anomaly | categorical | Normal / Drought Stress / Flood / Heat Stress |
| irrigation_needed | categorical | **Target** — Irrigate Now / Monitor / No Action |

### Target Variable Distribution

| Label | Meaning | Count |
|---|---|---|
| No Action | Soil moisture adequate | ~6,400 |
| Monitor | Borderline — watch closely | ~6,100 |
| Irrigate Now | Urgent irrigation needed | ~528 |

### Seasonal Patterns Applied

| Month | Condition |
|---|---|
| June – July | Early Northeast monsoon — high rainfall, high humidity |
| August – September | Peak monsoon — maximum soil moisture |
| October – November | Retreating monsoon — moisture starts dropping |

### Anomalies Injected (~4%)

| Type | Description |
|---|---|
| Drought Stress | Sudden soil moisture drop, zero rainfall |
| Flood/Overwatering | Moisture spike, excess rainfall |
| Heat Stress | Air and soil temperature spike |

---

## 🔍 Exploratory Data Analysis

| Analysis | Finding |
|---|---|
| Highest moisture crop | Rice (Thanjavur) — ~80% median soil moisture |
| Most irrigation alerts | Tomato (Coimbatore) — lowest base moisture |
| Peak rainfall months | July–August — Northeast monsoon effect |
| Anomaly rate | ~4% across all districts |
| Key correlation | Soil moisture strongly drives irrigation label |

---

## 🤖 Machine Learning Models

### 1. Random Forest Classifier
- **Task:** Predict irrigation need (3 classes)
- **Accuracy:** 95.65%
- **Key params:** n_estimators=50, max_depth=6, class_weight=balanced

### 2. SVM Classifier
- **Task:** Predict irrigation need (3 classes)
- **Accuracy:** 93.36%
- **Key params:** kernel=rbf, C=1.0, gamma=scale


### Model Comparison

| Model | Accuracy | F1 (weighted) |
|---|---|---|
| **Random Forest** | **95.65%** | **0.95** |
| SVM | 93.36% | 0.93 |

Random Forest outperformed SVM overall. However SVM showed competitive performance on majority classes. RF is recommended for deployment due to better recall on the critical **Irrigate Now** minority class.

### Feature Importance (Random Forest)

| Feature | Importance |
|---|---|
| soil_moisture | 55.3% |
| rainfall_mm | 21.2% |
| humidity_pct | 7.4% |
| nitrogen_mg_kg | 7.5% |
| air_temp_c | 4.1% |
| soil_temp_c | 4.3% |

Soil moisture emerged as the dominant predictor — consistent with agronomic decision-making where direct soil water content is the primary irrigation trigger.

---

## 💡 LIME Explainability

LIME (Local Interpretable Model-agnostic Explanations) was used to explain individual predictions — identifying which sensor readings most influenced each irrigation alert.

**Example explanation for a Monitor prediction:**
- soil_moisture low → +0.37 push toward irrigate alert
- rainfall_mm low → -0.09 offsetting factor
- nitrogen_mg_kg low → -0.10 offsetting factor
- Net result → Monitor (not urgent enough for Irrigate Now)

This bridges the gap between model accuracy and real-world trust — a farmer will act on an explained alert, not a black box prediction.

---

## 📈 Power BI Dashboard

The dashboard includes:
- **Page 1 — Overview:** KPI cards, irrigation donut chart, anomaly distribution, seasonal slicer
- **Page 2 — Sensor Analysis:** Soil moisture by crop, monthly trends, sensor summary table
- **Page 3 — Irrigation Alerts:** District-wise alert map, monthly alert trends
- **Page 4 — ML Results:** Model accuracy comparison, feature importance, confusion matrix

---

## ⚙️ How to Run

### Requirements
```
pip install pandas numpy matplotlib seaborn scikit-learn lime
```

### Steps
```bash
# Step 1 — Generate dataset
python simulation/generate_sensor_data.py

# Step 2 — Run EDA
python eda/eda_01_overview.py
python eda/eda_02_irrigation.py
# ... run each EDA script

# Step 3 — Run ML models
python ml/ml_01_random_forest.py
python ml/ml_02_svm.py
python ml/ml_03_isolation_forest.py
python ml/lime_explanation.py
```

---

## ⚠️ Limitations

- Dataset is simulated — real IoT sensor deployments would show stronger diurnal cycles and more complex inter-sensor dependencies
- Hourly patterns are flat due to simulation — real sensors show clear temperature peaks at 13:00–15:00h
- Class imbalance in Irrigate Now (~528 rows) — addressed with class_weight=balanced in RF
- Isolation Forest anomaly labels are unsupervised — ground truth validation would require real field data

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python | Data simulation, EDA, ML |
| pandas, numpy | Data manipulation |
| matplotlib, seaborn | Visualization |
| scikit-learn | ML models — RF, SVM|
| LIME | Model explainability |
| Power BI | Interactive dashboard |
| GitHub | Version control & portfolio |

---

## 👩‍💻 Author

**Narmadha V**


---

## 📄 License

This project is open source and available under the MIT License.
