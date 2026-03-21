# Multi-Store-SKU-Forecasting

## Overview
This project focuses on forecasting **3 months of sales** for **50 products** across **10 stores** of a company using **LightGBM**. Although this is a time series problem, machine learning methods were used due to their strong predictive performance.

The project follows a complete **CRISP-DM workflow**, including data preparation, feature engineering, model training, evaluation, and interpretation.

---

## Data and Time Series Handling
- Sales data is recorded daily for 50 items across 10 stores.
- **Chronological train-test split** was used to respect the time dependency of the data.
- **Expanding window time series cross-validation** was applied to avoid using the test set during model training, simulating a real production environment.
- The last **3 months** of data were reserved for testing and validation to reflect the reality of the problem.

---

## Feature Engineering
A thorough feature engineering process was carried out:
- **Date-related features**: month, day of week, day of year, etc.
- **Lag features**: past sales observations (e.g., 1 month, 3 months, 6 months, 1 year, 2 years)
- **Rolling window features**: mean, standard deviation, min, max
- **Exponentially weighted mean (EWM) features**
- **Log transformation** of the target variable (*sales*)

All features were tested with **time series cross-validation** to assess their contribution to model performance.

---

## Feature Selection
- **Recursive Feature Elimination (RFE)** was applied to select the most important features.
- The number of features was reduced from **85 to 31**, keeping the most predictive variables.

---

## Model Training and Hyperparameter Tuning
- **LightGBM** was chosen for its speed, predictive power, and robustness with minimal preprocessing.
- **Bayesian Optimization (Optuna)** was used to fine-tune hyperparameters on a representative subset of recent data (100,000 observations).
- Time series cross-validation ensured reliable performance evaluation.

---

## Model Performance
- **MAE**: 6.1 (predictions are off by 6.1 units on average)
- **RMSE**: 7.97
- **R²**: 92.2%
- **MAPE**: 13.28%
- Train, validation, and test errors are similar, demonstrating good generalization.

---

## Model Interpretation
- **SHAP values** and **LightGBM feature importances** were used to interpret the model.
- Most important features:
  - `month`, `dayofweek`, `item`, `dayofyear` (seasonality)
  - Rolling means (2 years, 1.5 years, 1 year)
  - Exponentially weighted mean features
- Certain rolling standard deviation features, like `sales_roll_std_365`, were also important.

---

## Forecasting Results
- **Total sales over 3 months**: 2,559,998 items
- **Average daily sales**: 27,527 items/day
- **Best performing stores**: 2, 3, 8  
- **Lowest performing stores**: 5, 6, 7
- **Most sold items**: 15, 28  
- **Least sold item**: 5

Predictions also include **best-case and worst-case scenarios**, accounting for possible errors.

---

## Key Insights
1. Time series feature engineering (lag, rolling, EWM, log transformation) significantly improves model performance.
2. Seasonal patterns (monthly and weekly) are crucial in predicting sales.
3. The model generalizes well to unseen data and provides actionable forecasts for individual stores and items.
4. Feature selection helps reduce complexity without sacrificing predictive power.

---

## Requirements
- Python >= 3.8
- pandas
- numpy
- scikit-learn
- lightgbm
- shap
- matplotlib
- seaborn
- optuna

---

## Usage
1. Clone the repository.
2. Install the required packages.
3. Run the notebooks or scripts for:
   - Data preparation and feature engineering
   - Model training and cross-validation
   - Forecast evaluation and interpretation

---

## License
This project is for educational and demonstration purposes. Please contact the author for any commercial usage.
