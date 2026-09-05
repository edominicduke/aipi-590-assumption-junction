# Team Name: AEJ (Still Not Fully Finished With Assignment - We Will Be Finished By Friday On The Deadline Posted On Canvas)

## Contributors

- Alberto — Linear Regression_
- Ethan — Logistic Regression_
- Jaideep — GAM

## Dataset

The Telco Customer Churn dataset (IBM sample data) has 7,043 customers from a telecom company, one row per customer. Each row has account info (contract type, payment method, tenure), billing (monthly/total charges), which services the customer has (phone, internet, streaming, security add-ons), and a `Churn` label saying whether that customer left the company. The task is to predict `Churn` from these attributes so the company can identify at-risk customers before they leave.

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| Linear regression | Multicollinearity, homoscedasticity, normality of residuals, influential outliers | Multicollinearity: found and fixed two issues (redundant "no service" categories, and MonthlyCharges/TotalCharges duplicating the service features), final VIF < 3 for every feature. Homoscedasticity: Breusch-Pagan p ≈ 0. Normality: Jarque-Bera p ≈ 0. Outliers: max Cook's Distance 0.0021 (well below the 1.0 concern threshold) | Homoscedasticity and normality both fail. These violations are expected with a binary target and limit the reliability of standard OLS inference, particularly conventional standard errors, p-values, and confidence intervals |
| Logistic regression | Binary outcome, Independence of observations, No perfect separation, Linear log-odds, Limited multicollinearity, and Sufficient sample size | Binary outcome: The target (Churn) only has two values (0 and 1); Independence of observations: No customers appear more than once and no completely duplicated observations were found;  No perfect separation: No category has observations from only one outcome class as determined by pd.crosstab; Linear log-odds: The plot of the log-odds of churn vs tenure (the only continuous variable) is a diagonal line (demonstrating linearity in the log-odds of churn with respect to tenure); Limited multicollinearity: The highly correlated features TotalCharges and MonthlyCharges were removed (see notebook for more information)  | No concerns given the passing of all of the assumptions |
| GAM | _[fill in]_ | _[fill in]_ | _[fill in]_ |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| Linear regression | R² = 0.26, RMSE = 0.38 on the test set; 79.8% accuracy when predictions are thresholded at 0.5 | Coefficients are very easy to explain: each one is a direct change in churn probability, in percentage points, no extra math needed for a non-technical audience | About 17% of predictions fall outside the valid [0, 1] probability range (down to about -24%), so the raw outputs are not valid probabilities and are unsuitable as calibrated risk scores |
| Logistic regression | Accuracy on Test Set = 82.11% | Coefficients can easily explain how each feature impacts the log-odds of churn (a  a one unit increase in SeniorCitizen results in a 0.16312020439311975 increase in the log-odds of churn) | Might be difficult for a non-technical audience to understand what it means for a variable to affect the "log-odds" of another variable (in this case churn) |
| GAM | _[fill in]_ | _[fill in]_ | _[fill in]_ |

## Recommendation

**Recommended model:** _[fill in once all three models are compared]_

**Why this model:** _[fill in]_

**What the company can responsibly conclude:** _[fill in]_

**What the company should not conclude yet:** _[fill in]_

**One next analysis we would run:** _[fill in]_
