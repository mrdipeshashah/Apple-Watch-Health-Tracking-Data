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

## FORECASTING 90-DAY VIEW
Using Big Query utilizing a **90-day rolling look-back period** to look into the future. 

90-day lookback was selected for 2 reasons: 

1) Relevancy - 90 day windows captures the most recent trends and ensures forecasts are releastic and attainable. Looking back at 6 months of data is a poorer predictor in fitness. THe most recent is the most accurate which is why 90-days is the sweet spot
2) Volatility - Using the 90-day average the forecast is not massively impacted by a massive spike when travelling or a netflix and chill day
   
* **Step Baseline** (**Forecasted Step Count**) - The predicted volume required to maintain current fitness based on the last 3 months of behavior.
* **Step Count** (**Actual Step Count**) - The step count that was inputted into the Google Sheet 
* **Step Stretch Goal (5% Growth)** - A +5% "Progressive Overload" target. Consistently hitting this goal "pulls" the baseline upward over time, expanding the engine's total capacity

How the data gets updated into Google Sheets can impact the forecasting: There are 2 routes - Real Time v Fixed Benchmarking 

1) Fixed Benchmarking - To understand April data, only upload April's data on 1st May meaning the forecasted data for April which is based on January to March is frozen
2) Real Time - If April's data is updated on 16th April for the first 15 days that will change the forecast for April 

## DATA GLOSSARY

This project utilizes two primary BigQuery views to separating real-time efficiency metrics from forecasting. Below is the breakdown of every dimension and metric used in the model.

### **1. Core Dimensions (Shared Logic)**
| Field | Definition |
| :--- | :--- |
| **Date** | The primary calendar key for each activity entry. |
| **year_number** | Numeric year for long-term annual trend analysis. |
| **month_number** | **Crucial Sorting Field:** Prevents alphabetical sorting (April before August). |
| **month_name** | The display label for charts (e.g., "April", "May"). |
| **month_year** | Unique string (YYYY-MM) used to keep a continuous timeline across multiple years. |
| **week_number** | Groups data into 7-day cycles to identify weekly habit patterns. |
| **Gym_YN** | A Boolean flag identifying days with intentional workout sessions. |
| **activity_segment** | Behavioral classification (e.g., "Gym Routine", "Travelling") based on step volume. |
| **data_segment** | Distinguishes between **"Actuals"** (past data) and **"Planned/Future"** (forecasted dates). |
| **lifestyle_era** | Qualitative label to track major life phases (e.g., "Post-Pandemic") for historical context. |

### **2. Efficiency & Actuals (`base_view`)**
| Field | Definition |
| :--- | :--- |
| **gym_count** | A running tally of total gym sessions within a specific period. |
| **is_high_travel_day** | A binary flag for days exceeding 20k steps; used to isolate outliers from training data. |
| **Efficiency_MPG** | Cardiovascular Economy: Calculates physical output per heartbeat (`Steps / Heart Rate`). |
| **Capacity_Score** | The Performance Index: Standardizes daily efficiency into a 30-point scale for visual indexing. |

### **3. Forecasting & Targets (`actual_v_targets`)**
| Field | Definition |
| :--- | :--- |
| **actual_distance** | Real-world kilometers covered based on Apple Watch GPS/pedometer data. |
| **capacity_actual** | The calculated capacity score for days where active data exists. |
| **step_baseline** | Maintenance Forecast: The 90-day rolling average of step volume. |
| **step_stretch_goal** | Growth Forecast: Baseline + 5% target for progressive overload. |
| **distance_baseline** | The 90-day rolling average of total distance (km) covered. |
| **distance_stretch** | Distance Baseline + 5% target for endurance growth. |
| **capacity_baseline** | The 90-day rolling average of your Efficiency MPG x 30. |
| **capacity_stretch** | Capacity Baseline + 5% target for cardiovascular system growth. |
| **capacity_variance** | The Performance Gap: The delta between current output and the forecasted baseline. |

## DASHBOARD 

### MY HEALTH INSIGHTS

This dashboard covers my apple watch health data from January 2024 onwards

https://datastudio.google.com/reporting/803e5c48-fb6d-4721-8dad-9ee0ab49e25f

### DASHBOARD TEMPLATE

A copy of the dashobard can be made and linked to the Big Query views 

https://datastudio.google.com/reporting/e9e46c3e-8e59-4690-b8bb-afdf3c4f2c2c

## DASHBOARD VISULISATION

The dashboard is structured into three layers: 

### **1-High-Level Performance**

### **The Scoreboard**
#### **Monthly Volume Step Count Distribution**
* **Focus:** Total absolute step volume per month.
* **Insight:** Provides a macro view of physical activity across years to identify seasonal trends and long-term volume growth.

#### **Training Consistency: Monthly Gym Frequency**
* **Focus:** Tracks the number of intentional workout sessions completed each month.
* **Insight:** Acts as a lead indicator for performance; higher gym frequency typically precedes surges in cardiovascular efficiency.

#### **Yearly Performance (The Master Table)**
* **Focus:** A historical heat map of Step Count, Gym Count, Capacity Score, and Efficiency MPG.
* **Insight:** Provides an at-a-glance comparison of annual performance, highlighting 2025 as a peak efficiency year with a record 201 gym sessions.

### **2-The Forecasting Runway**

#### **Actual vs. Forecasted Step Count**
* **Focus:** Maps real-time daily output against the 90-day rolling baseline.
* **Insight:** Extends the timeline through December 2026, visualizing the "Maintenance Runway" to show required volume for future months.

#### **The Performance Gap**
* **Focus:** A targeted look at the variance between current monthly output and the baseline/stretch goals.
* **Insight:** Immediately identifies if the "Engine" is currently in a state of growth (hitting targets) or recovery (falling below baseline).

### **3-Biometric Efficiency**

### **Biometric Efficiency (The "Human Engine")**
#### **Efficiency Over Time (MPG & Capacity)**
* **Focus:** Tracks the relationship between Efficiency MPG (Economy) and Capacity Score (Volume).
* **Insight:** Visualizes how improvements in cardiovascular health allow for higher work volumes without increased physiological strain.

#### **Average Steps per Gym Session vs. Efficiency**
* **Focus:** Correlates training intensity with aerobic ROI.
* **Insight:** Demonstrates that the 19% efficiency surge in 2025 was directly driven by maintaining a high intensity of ~9k steps per session.

#### **Efficiency v Intensity Mapping (The Sweet Spot)**
* **Focus:** A bubble chart identifying the intersection of high mechanical output and low heart rate.
* **Insight:** Pinpoints the "Efficiency Ceiling"—the exact intensity level where the body operates at its most economical state.

#### **The Efficiency View (Pre vs. Post Strategy)**
* **Focus:** Comparative scatter plot mapping heart rate against step count.
* **Insight:** Visualizes the "shift" in performance, proving that newer training strategies have moved the cluster toward higher volume at lower relative heart rates.

#### **Behavioral Segmentation (Pre and Post Stretching)**
* **Focus:** Stacked bar chart categorizing all activity into tiers (Gym, Exploring, Travelling, etc.).
* **Insight:** Shows the "Activity Mix" change over time, highlighting how intentional gym sessions have become the dominant driver of total volume.
