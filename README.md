# 🏃‍♂️ The Human Engine: Predictive Health Modeling
Transforming Apple Watch health data into a dynamic performance engine using SQL, BigQuery, and Looker Studio.

## 📌 Project Overview
This project reframes personal health tracking as "Engine Management." By treating physical activity as output and heart rate as internal stress, this system creates a **90-day predictive model** to forecast future physical requirements and measure cardiovascular efficiency through December 2026.

## 🧬 The "Biological KPI" Suite
This project utilizes three proprietary metrics to distinguish between mere "movement" and "performance."

### 1. Efficiency MPG (Steps per Heartbeat)
* **Formula:** `Total Steps / Average Heart Rate`
* **Concept:** This measures the body’s "fuel economy." 
* **Goal:** Move more while keeping the heart rate lower. A higher number indicates a more "economical" and athletic cardiovascular system.

### 2. Capacity Score (Total Work Output)
* **Formula:** `Efficiency MPG x 30` (Standardized Multiplier)
* **Concept:** This translates the "Economy" of the body into a "Performance Index." 
* **The Multiplier:** A factor of x30 creates a visual scale that clearly distinguishes "Peak" performance days from "Maintenance" days. It provides a standardized daily "Work Volume" score.

### 3. Intensity Context (Avg Steps per Session)
* **The "Turbocharger" Metric:** While Capacity Score measures total volume, **Avg Steps per Session** measures **Intensity**. 
* **Insight:** Capacity Score alone lacks context. High-value insights come from pairing volume with intensity—proving the engine isn't just running longer, but harder during training sessions.

---

## 📈 The Forecasting Engine
The dashboard utilizes a **90-day rolling window** to project benchmarks into the future. 

* **Step Baseline (Maintenance):** The predicted volume required to maintain current fitness based on the last 3 months of behavior.
* **Step Stretch Goal (Growth):** A +5% "Progressive Overload" target. Consistently hitting this goal "pulls" the baseline upward over time, expanding the engine's total capacity.

---

## 🛠 Tech Stack & Transformation
* **Data Source:** Apple Watch (Health Auto Export)
* **Warehouse:** Google BigQuery
* **Visualization:** Looker Studio
* **Sorting Logic:** The model utilizes `month_year` and `month_number` to ensure chronological accuracy across multi-year data sets.

