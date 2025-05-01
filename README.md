# Marketing Mix Model (MMM) Analysis - Refined

## Project Overview

This project performs an end-to-end Marketing Mix Modeling (MMM) analysis using weekly marketing and sales data provided in `marketing.csv`. The goal is to understand the impact of various marketing channels, pricing, promotions, and other factors on Gross Merchandise Value (GMV) and provide data-driven insights for potential marketing optimization.

## Objective

* Quantify the contribution of different marketing levers and external factors to weekly GMV.
* Calculate the elasticity of GMV with respect to significant drivers.
* Provide reliable, data-driven insights to inform marketing strategy.

## Data

The analysis uses `marketing.csv`, containing weekly data on GMV, marketing spend, pricing, operational metrics, KPIs, and event flags.

## Methodology & Refinement Process

The analysis involved standard MMM steps, including data preparation, feature engineering (Adstock, Moving Averages, Lags), EDA, and regression modeling (Linear, Multiplicative, Koyck, Distributed Lag).

### Challenges Faced

* **Severe Multicollinearity:** Initial model runs (Linear, Multiplicative, Koyck, DLM) exhibited extremely high Variance Inflation Factor (VIF) scores (many >> 10, some in the hundreds/thousands), particularly among related features like different Moving Average terms, lagged variables, and some marketing adstock variables. This indicated strong correlations between predictors.
* **Implausible Coefficients:** As a likely consequence of multicollinearity, initial models produced statistically significant coefficients with signs contrary to business logic (e.g., major marketing channels like TV, Digital, SEM showing a significant *negative* impact on GMV).
* **Unreliable Estimates:** High VIFs and implausible coefficients meant the initial model results were unstable and could not be reliably used for interpretation or elasticity calculation.

### Changes Made (Refinement Strategy)

To address these challenges, the modeling process was refined:

1.  **Selective Feature Input:** Reduced the number of potentially collinear features used as *initial* input for stepwise selection. Specifically:
    * Used only the 4-week Moving Average inflation terms (`MA4_list_price_inf`, `MA4_discount_inf`) instead of multiple window sizes (2, 3, and 4 weeks).
    * Initially included only lag 1 for key predictors instead of lags 1, 2, and 3.
    * Excluded `Total_Investment` as individual channel adstocks were present.
2.  **Iterative VIF & P-value Checks:** Implemented a more robust feature removal strategy *after* the initial stepwise selection:
    * Calculated VIFs for the features selected by stepwise regression.
    * Iteratively removed features that had **both** a high VIF (threshold > 5.0) **and** were statistically insignificant (p-value > 0.05).
    * Prioritized removing the feature with the highest VIF among those meeting both criteria in each iteration.
    * If no feature met both criteria, considered removing the feature with the highest VIF *only if* its p-value was also insignificant. The process stopped if the highest VIF feature was statistically significant.

### Reaching the Final Result

1.  **Model Re-building:** The Linear, Multiplicative, Koyck, and Distributed Lag models were rebuilt using the refined feature selection process.
2.  **Evaluation:** The refined models were evaluated based on:
    * Adjusted R-squared (goodness of fit)
    * VIF scores (checking for resolved multicollinearity - target < 5)
    * P-values (statistical significance of predictors)
    * Coefficient signs (business logic plausibility)
    * Durbin-Watson statistic (checking for residual autocorrelation)
3.  **Model Comparison & Selection:** The refined models were compared. While none were perfect, the multicollinearity was significantly reduced. The **Refined Linear Model** was chosen as a reasonable balance, exhibiting excellent VIFs, plausible coefficients for key drivers (discount, stock index, promotions, price sensitivity), and at least one significant positive marketing channel (Affiliates), although some challenges remained (e.g., insignificant major channels, negative Radio coefficient). The Koyck model showed a slightly better statistical fit but was less interpretable due to the dominance of lagged GMV and remaining negative coefficients.
4.  **Elasticity Calculation:** Elasticities were calculated for the chosen refined model (Linear) to quantify the impact of significant drivers.

## How to Run

1.  Ensure Python and necessary libraries (`pandas`, `numpy`, `matplotlib`, `seaborn`, `statsmodels`, `scikit-learn`) are installed.
2.  Place `marketing.csv` in the same directory as `Synthetic_MMM_Analysis.ipynb`.
3.  Run the notebook cells sequentially in a Jupyter environment.

## Key Libraries Used

* pandas
* numpy
* matplotlib
* seaborn
* statsmodels
* scikit-learn

