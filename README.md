# AgriSense TN — IoT Crop Field Monitoring + Agentic Alerts

Simulated IoT crop-monitoring system for Tamil Nadu agricultural fields — combining classical ML, explainable AI, and an agentic decision-to-action loop that turns model predictions into farmer-facing SMS alerts.

## Overview

Real-time IoT sensor data from Tamil Nadu farms isn't publicly available, so this project simulates hourly field sensor readings (soil moisture, temperature, rainfall, nitrogen, humidity) for three crop/district pairs, then builds a full pipeline from raw data → prediction → explanation → **autonomous alert**.

The project has two layers:
1. **ML layer** — predicts irrigation need from sensor data, with model explainability
2. **Agentic layer** — decides whether an alert is needed and sends it automatically, closing the gap between "model output" and something a farmer can actually act on

## Dataset

- **Type:** Simulated IoT time-series data
- **Region:** Tamil Nadu, India — Thanjavur (Rice), Erode (Turmeric), Coimbatore (Tomato)
- **Period:** June – November 2024, hourly readings
- **Size:** 13,107 rows × 11 columns
- **Basis:** Generated from IMD (India Meteorological Department) seasonal climate patterns and ICAR agronomic guidelines, with injected noise and anomalies to mimic real field conditions

**Target:** `irrigation_needed` — `No Action` / `Monitor` / `Irrigate Now`

**Features:** `soil_moisture`, `soil_temp_c`, `air_temp_c`, `humidity_pct`, `rainfall_mm`, `nitrogen_mg_kg`

## Pipeline

### 1. Data Simulation & EDA
- Synthetic sensor generation with realistic noise (prevents trivial 100% accuracy)
- Crop-wise distribution analysis, monthly trends, correlation heatmaps, anomaly detection, diurnal patterns

### 2. Modeling
| Model | Accuracy |
|---|---|
| Random Forest | 95.65% |
| SVM (RBF kernel) | 93.71% |

### 3. Explainability
- **Feature importance** (Random Forest) and **permutation importance** (SVM) — soil moisture consistently ranks as the top predictor, mirroring how a farmer would actually reason
- **LIME** — per-prediction, human-readable explanations for individual field readings

### 4. Agentic Layer
The ML model predicts; the agent decides what to do about it and acts:

```
Perceive → sensor reading (soil moisture, rainfall, nitrogen, etc.)
Decide   → RF model predicts label + rule-based reasoning generates a plain-language "why"
           → No Action stays silent (avoids alert fatigue)
Act      → Claude API turns the decision into an SMS-style message → sent via WhatsApp/Twilio
Observe  → outcome logged for the next monitoring cycle
```

This is what separates it from a static dashboard: the system decides *and* acts, instead of waiting for someone to open a notebook and read a chart.

## Tech Stack

`Python` · `pandas` / `numpy` · `scikit-learn` · `XGBoost` · `LIME` · `Claude API (Anthropic)` · `Twilio (SMS)` · `Power BI` · `matplotlib` / `seaborn`



## Author

Narmadha — [LinkedIn / GitHub links]

## Disclaimer

Sensor data is synthetically generated for demonstration purposes and does not represent real-time measurements from actual Tamil Nadu farms.
