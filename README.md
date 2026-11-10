# Can We Beat "Same as Today"? Predicting Chicago's Weather

## Overview
This project explores whether Machine Learning can accurately predict daily weather (specifically temperature and precipitation) in Chicago, using historical data from 2009. The core question driving this analysis is: **"Can we predict tomorrow's weather from today's data, and does a complex AI model actually beat a simple 'same as today' baseline?"**

## The Data
*   **Source:** ERA5 daily means over the continental US.
*   **Variables:** Temperature, Precipitation, Pressure, and Wind Speed.
*   **Timeframe:** 2009 to 2010 (730 days).
*   **Spatial Resolution:** A 105 × 237 grid.

## Testing Methodology (Preventing Data Leakage)
Weather changes slowly; the weather today is nearly identical to the weather yesterday. If we used a random train/test split, the model would "cheat" by memorizing neighboring days rather than learning actual weather patterns.
*   **Train Set:** All of 2009 (Past).
*   **Test Set:** All of 2010 (Future).
*   **Metric:** Mean Absolute Error (MAE). *Lower is better.*

## Exploratory Data Analysis (EDA)
*   **Temperature:** Follows a smooth, predictable seasonal wave. It is highly dependent on latitude (warmer in the South, colder in the North).
*   **Precipitation:** Highly chaotic and localized. It is "bursty," consisting mostly of zero values interrupted by sudden spikes (storms). 
*   **Smoothing:** We utilized 3-day and 7-day rolling averages to remove daily noise and reveal the true underlying weather trends.

## The Models Tested
We evaluated several models to establish baselines and find the best predictor:
1.  **Mean Baseline:** Always predicts the 2009 average.
2.  **Persistence Baseline ("Same as Today"):** Assumes tomorrow will be exactly like today.
3.  **Linear Regression:** A simple, standard Machine Learning model.
4.  **Neural Network (MLPRegressor):** A more complex Deep Learning model.
5.  **Random Forest & Gradient Boosting:** Advanced tree-based models (for multi-day forecasts).

## Key Findings & Results

### 1. Temperature is Predictable
*   **The Baseline:** "Same as today" is hard to beat, yielding a 1.64°C error.
*   **The AI Failure:** A simple Linear model using *only* today's temperature scored a 1.67°C (worse than the baseline).
*   **The Breakthrough:** By feeding the model 7-day rolling averages, other weather variables (pressure/wind), and **spatial neighbors** (data from surrounding regions to track moving storms), we achieved our best score: **1.52°C MAE**.
*   **The Neural Network Trap:** The Neural Network actually performed *worse* (2.02°C) due to **overfitting**. It was too complex and memorized the noise of 2009 instead of learning general rules. 

### 2. The 5-Day Forecast Showdown
*   We tested predictions 1 to 5 days into the future.
*   **Winner:** Linear Regression remained the most accurate model across all 5 days (ending at 2.90°C MAE).
*   **Loser:** The Neural Network performed the worst at 5 days out. *Lesson: Complexity does not always equal accuracy.*

### 3. Rain is Chaotic
*   For a 1-day forecast, Linear Regression was the best (0.164 mm MAE).
*   However, beyond a 2-day forecast, the AI could not beat the "Mean" baseline (guessing the historical average). Rain is simply too random for this dataset to predict further out.

### 4. Extreme Heat is Easy to Predict
*   We reframed the problem to a classification task: "Will tomorrow be a 'Hot Day'?" (defined as >19.2°C).
*   Using AUC (Area Under the Curve) as the metric, all models scored incredibly high (**~0.989**). Predicting extreme heat is vastly easier than predicting exact temperatures or rain.

## Conclusion
1.  **Context is King:** Simple Linear models win when given the right features (rolling averages and spatial data).
2.  **Complexity is a Trap:** Neural Networks often overfit small, chaotic datasets like this one. 
3.  **Different Variables, Different Rules:** Temperature is smooth and predictable; rain is chaotic and nearly impossible to predict beyond 48 hours.

## Built With
*   **Python**
*   **Pandas & NumPy** (Data manipulation)
*   **Scikit-Learn** (Machine Learning models & metrics)
*   **Matplotlib** (Data visualization)

---
*Created by Mathew Yeung*
