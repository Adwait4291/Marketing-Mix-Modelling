# Marketing Mix Model (MMM) Analysis

## Project Overview

This project performs an end-to-end Marketing Mix Modeling (MMM) analysis using weekly marketing and sales data provided in `marketing.csv`. The goal is to understand the impact of various marketing channels (TV, Digital, SEM, etc.), pricing, promotions, and other factors on Gross Merchandise Value (GMV).

The analysis replicates the methodology demonstrated in the `Market_Mix_Model_ElecKart.ipynb` example notebook.

## Objective

* Quantify the contribution of different marketing levers and external factors to weekly GMV.
* Calculate the elasticity of GMV with respect to significant drivers (marketing spend, price, promotions).
* Provide data-driven insights to potentially inform future marketing budget allocation and strategy.

## Data

The primary input data for this analysis is `marketing.csv`, which contains weekly data including:
* `gmv`: Gross Merchandise Value (Target Variable)
* Marketing spend across various channels (TV, Digital, Sponsorship, Content, Online, Affiliates, SEM, Radio, Other)
* Pricing information (`listing_price`, `product_mrp`, `discount`)
* Operational metrics (`sla`, `product_procurement_sla`)
* Other KPIs (`NPS`, `Stock_Index`)
* Event flags (`Special_sales`, `Payday`)

## Methodology

The analysis follows these key steps:

1.  **Data Loading & Preparation:** Loads the `marketing.csv` data and performs necessary type conversions (e.g., Date column).
2.  **Feature Engineering:**
    * Calculates **Adstock** for marketing spend variables using a geometric decay model to account for carryover effects.
    * Computes **Moving Averages** and **Inflation/Difference** variables for pricing and discount features.
    * Creates **Lagged Variables** for key predictors and the target variable (GMV) to capture delayed effects.
3.  **Exploratory Data Analysis (EDA):** Visualizes GMV trends and analyzes correlations between variables.
4.  **Modeling:** Builds and evaluates several regression models:
    * Linear Model
    * Multiplicative (Log-Log) Model
    * Koyck Model (with lagged GMV)
    * Distributed Lag Model (with lagged predictors)
    * Uses **Stepwise Selection** (forward-backward based on p-values) to refine features for each model type.
    * Evaluates models based on R-squared, Adjusted R-squared, p-values, and VIF scores.
5.  **Elasticity Calculation:** Calculates and visualizes the elasticity of GMV for significant predictors in the final chosen model.

## How to Run

1.  Ensure you have Python and the necessary libraries installed (`pandas`, `numpy`, `matplotlib`, `seaborn`, `statsmodels`, `scikit-learn`).
2.  Make sure the `marketing.csv` file is in the same directory as the Jupyter Notebook (`Synthetic_MMM_Analysis.ipynb`).
3.  Open and run the `Synthetic_MMM_Analysis.ipynb` notebook in a Jupyter environment (like Jupyter Lab or Jupyter Notebook). The notebook will load the data, perform the analysis, and display the results, including model summaries and elasticity plots.

## Key Libraries Used

* pandas
* numpy
* matplotlib
* seaborn
* statsmodels
* scikit-learn

