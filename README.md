# Team Name: AEJ

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
| Logistic regression | _[fill in]_ | _[fill in]_ | _[fill in]_ |
| GAM | _[fill in]_ | _[fill in]_ | _[fill in]_ |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| Linear regression | R² = 0.26, RMSE = 0.38 on the test set; 79.8% accuracy when predictions are thresholded at 0.5 | Coefficients are very easy to explain: each one is a direct change in churn probability, in percentage points, no extra math needed for a non-technical audience | About 17% of predictions fall outside the valid [0, 1] probability range (down to about -24%), so the raw outputs are not valid probabilities and are unsuitable as calibrated risk scores |
| Logistic regression | _[fill in]_ | _[fill in]_ | _[fill in]_ |
| GAM | _[fill in]_ | _[fill in]_ | _[fill in]_ |

## Recommendation

**Recommended model:** _[fill in once all three models are compared]_

**Why this model:** _[fill in]_

**What the company can responsibly conclude:** _[fill in]_

**What the company should not conclude yet:** _[fill in]_

**One next analysis we would run:** _[fill in]_
