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

**Notes** - Tested out the multiplier first of x 20 then x 30. Using a multiplier of x 30 for Capacity Score provided a metric to visualise total output on a scale that allows to see good v average days turning into a performance index. 

### 3. Intensity Context (Avg Steps per Session)
* **The "Turbocharger" Metric:** While Capacity Score measures total volume, **Avg Steps per Session** measures **Intensity**. 
* **Insight:** Capacity Score alone lacks context. High-value insights come from pairing volume with intensity—proving the engine isn't just running longer, but harder during training sessions.



## The Forecasting Engine
Using Big Query utilizing a **90-day rolling look-back period** to look into the future. 

90-day lookback was selected for 2 reasons: 

1) Relevancy - 90 day windows captures the most recent trends and ensures forecasts are releastic and attainable. Looking back at 6 months of data is a poorer predictor in fitness. THe most recent is the most accurate which is why 90-days is the sweet spot
2) Volatility - Using the 90-day average the forecast is not massively impacted by a massive spike when travelling or a netflix and chill day 

In the Google Sheet, it requires extending out the dates i.e. in April 2026 - extending the dates to end July 2026 which covers the 90-day rolling look back period. Extending the dates to end December 2026 can be done but it will only cover a 90-day look-back from the last available date inputted into the Google sheet. 

* **Step Baseline** (**Forecasted Step Count**) - The predicted volume required to maintain current fitness based on the last 3 months of behavior.
* **Step Count** (**Actual Step Count**) - The step count that was inputted into the Google Sheet 
* **Step Stretch Goal (5% Growth)** - A +5% "Progressive Overload" target. Consistently hitting this goal "pulls" the baseline upward over time, expanding the engine's total capacity.

## 📖 Data Glossary

### **Core Dimensions**
| Field Name | Description |
| :--- | :--- |
| **Date** | The primary calendar key for activity tracking. |
| **month_number** | Numeric month (1-12) used to force chronological sorting in visualizations. |
| **month_year** | Formatted string (YYYY-MM) to ensure timeline continuity across multiple years. |
| **activity_segment** | Behavioral classification (e.g., "Gym Routine", "Travelling") based on step volume. |

### **The Efficiency Model (`base_view`)**
| Field Name | Description |
| :--- | :--- |
| **is_high_travel_day** | Binary flag identifying high-volume step outliers (>20k steps). |
| **Efficiency_MPG** | Cardiovascular Economy: `Total Steps / Average Heart Rate`. |
| **Capacity_Score** | Standardized performance index using a 30x multiplier on Efficiency MPG. |

### **The Forecasting Model (`actual_v_targets`)**
| Field Name | Description |
| :--- | :--- |
| **step_baseline** | Maintenance Forecast: The 90-day rolling average of step volume. |
| **step_stretch_goal** | Growth Forecast: Baseline + 5% target for progressive overload. |
| **capacity_actual** | Real-time performance score for active tracking days. |
| **capacity_variance** | The performance gap: Delta between `capacity_actual` and `capacity_baseline`. |

