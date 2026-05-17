# Budapest Airbnb Pricing Analysis
## Overview 
This repository contains a comprehensive statistical analysis of the short-term rental market in Budapest. The primary objective of this project was to determine whether renting an Airbnb in Budapest is statistically more expensive on weekends compared to weekdays, while controlling for structural, locational, and quality-based covariates.

Using a dataset of over 4,000 listings, this project employs a Multiple Linear Regression (MLR) framework built in R to uncover the true drivers of pricing dynamics in the Budapest hospitality market. [The original dataset can be found here](https://www.kaggle.com/datasets/thedevastator/airbnb-prices-in-european-cities?select=budapest_weekdays.csv)

## Key Findings 
Based on the final model evaluated on an unseen test set (holding all other factors constant):
- Weekend bookings command an 8.24% price premium compared to weekday bookings (p < 0.01)
- Each additional guest capacity increases the price by 8.62%, and each additional bedroom adds 5.76%
- For every 1-unit increase in distance from the city center, the expected price decreases by 2.77%
- Renting a "Private room" is 33.7% cheaper on average than renting an "Entire home/apt"

## Data 
The analysis is based on a dataset of 4,022 Airbnb listings. Key variables utilized in the final analysis include:
- realSum: The total price of the listing in EUR (Target Variable)
- day_type: Categorical indicator of Weekday vs. Weekend
- room_type: Categorical indicator (Entire home/apt, Private room, Shared room)
- person_capacity: Maximum guest capacity
- bedrooms: Number of bedrooms
- dist: Distance from city center
- mult: : Boolean indicating if the listing is for multiple rooms

## Methodology
1. Data Preprocessing & EDA
- Log Transformation: The target variable realSum exhibited severe right-skew and was transformed using the natural logarithm (log_price) to stabilize variance and satisfy normality assumptions.
- Multicollinearity Pruning: Highly correlated variables (e.g., metro_dist and guest_satisfaction_overall) were preemptively dropped to prevent variance inflation.

2. Model Selection
- Data Splitting: The dataset was split into an 80% training set and a 20% test set
- Backward Selection: A rigorous backward selection algorithm was applied to the training set. Due to the large sample size (n > 3,200), a strict threshold proxy (p < 0.001) was used during this phase to effectively penalize model complexity and drop practically insignificant variables.

  
3. Model Diagnostics & Validation
- Verified core assumptions of Multiple Linear Regression on the training model:
    - Linearity & Homoscedasticity: Confirmed via Residuals vs. Fitted and Scale-Location plots.
    - Normality: Confirmed via Normal Q-Q plots (with minor expected deviations at the extreme right tail due to luxury listings).
    - Influential Outliers: Cook's Distance boundaries were respected in the Residuals vs. Leverage plot.
    - VIF: All Variance Inflation Factors were strictly below 5, confirming the absence of multicollinearity.
      
4. Final Inference
The final finalized model structure was fitted exclusively on the 20% holdout test dataset to generate unbiased coefficient estimates and evaluate final statistical significance at alpha = 0.05.

## Tools and Libraries Used
- R
- tidyverse
- repr
- rsample
- ggcorrplot
- car
