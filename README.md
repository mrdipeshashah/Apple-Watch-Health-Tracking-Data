# PREDICTIVE HEALTH MODELLING
Transforming Apple Watch health data to provide richer insights with using the Google Ecoysystem - Google Sheets + Big Query + Data studio 

## OVERVIEW
Apple Health provides some good data but it's also quite limiting as well, I wanted to dig into the data more and surfass richer insights to understand the impact of my gym sessions which is very much linked to my strectching I do at least once a month. The common view is to do 10K steps a day which is a great benchmark to have but it was not giving me the insights I wanted. The goal was to move away from step count which I was using as the success metric.  

## THE PROCESS 
The data collection prcoess is the biggest step and getting it right is critical. The process post the data collection should be smoother. 

1) To get the data exported out of Apple Watch it requires using Health Auto Export (or similar) - https://apps.apple.com/us/app/health-auto-export-json-csv/id1115567069 it provides the ability to get the data into CSV.

   The core metircs that need to be tracked at a daily level: 
   * Step Count
   * Heart Rate
   * Distance (KM)

2) Then get the data into Google Sheets using this template (do not change headers) - https://docs.google.com/spreadsheets/d/1rJIg44mvjqyTCQPpO0dZ13MtRhzehohWx9Nis5gkFgs/edit?usp=sharing 

   The key addtional requirement in the Google Sheet is the tracking if you have visited the Gym, which is tracked as Y(Yes) and N(No). 

3) It also requires having Big Query setup. Creating a dataset & table (using the Google Sheet)

## KEY KPI'S TO TRACK PERFORMANCE 
From the data available in the Google Sheet, I have developed three custom KPI's to better measures cardiovascular from 'movement' to 'performance' 

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


## The Forecasting Engine
Using Big Query it utilizes a utilizes a **90-day rolling look-back period** to look into the future. 

* **Step Baseline (Maintenance):** The predicted volume required to maintain current fitness based on the last 3 months of behavior.
* **Step Stretch Goal (Growth):** A +5% "Progressive Overload" target. Consistently hitting this goal "pulls" the baseline upward over time, expanding the engine's total capacity.

## 🛠 Tech Stack & Transformation
* **Data Source:** Apple Watch (Health Auto Export)
* **Warehouse:** Google BigQuery
* **Visualization:** Looker Studio
* **Sorting Logic:** The model utilizes `month_year` and `month_number` to ensure chronological accuracy across multi-year data sets.

