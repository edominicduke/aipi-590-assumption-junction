# Team Name: AEJ

## Contributors

- Alberto: Linear Regression
- Ethan: Logistic Regression
- Jaideep: Generalized Additive Model (GAM) and final model comparison

## Dataset

The Telco Customer Churn dataset (IBM sample data) has 7,043 customers from a telecom company, one row per customer. Each row has account info (contract type, payment method, tenure), billing (monthly/total charges), which services the customer has (phone, internet, streaming, security add-ons), and a `Churn` label saying whether that customer left the company. The task is to predict `Churn` from these attributes so the company can identify at-risk customers before they leave.

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| Linear regression | Multicollinearity, homoscedasticity, normality of residuals, influential outliers | Multicollinearity: found and fixed two issues (redundant "no service" categories, and MonthlyCharges/TotalCharges duplicating the service features), final VIF < 3 for every feature. Homoscedasticity: Breusch-Pagan p ≈ 0. Normality: Jarque-Bera p ≈ 0. Outliers: max Cook's Distance 0.0021 (well below the 1.0 concern threshold) | Homoscedasticity and normality both fail. These violations are expected with a binary target and limit the reliability of standard OLS inference, particularly conventional standard errors, p-values, and confidence intervals |
| Logistic regression | Binary outcome, independent observations, limited multicollinearity, sufficient sample size, no perfect separation, and linearity in the log-odds | The target has two classes, each customer appears once, no missing values remained after cleaning, the model converged, and no obvious separation candidates were found. MonthlyCharges and TotalCharges were removed because they were highly correlated with other predictors. Tenure, the remaining continuous predictor, was checked against the log-odds of churn. | Removing the charge variables reduced multicollinearity but also removed potentially useful information. The model also does not include nonlinear effects or interactions unless they are added directly. |
| GAM | Binary outcome, independent observations, coverage across predictors, category support, and possible concurvity | After cleaning, 7,032 unique customers remained and the churn rate was 26.6%. Each factor level had support, and the train/test split was stratified. The strongest continuous-predictor correlation was 0.889. Smoothing penalties were selected using the training data only. | Tenure and TotalCharges are strongly related, so their smooth effects may divide the same signal. The model is additive and may miss interactions. Results should be treated as associations, not causal effects. |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| Linear regression | Test R² = 0.263, RMSE = 0.379, accuracy = 79.8%, and churn recall = 50.5% at a 0.5 threshold | Coefficients can be explained directly as changes in predicted churn probability. | About 16.8% of test predictions fall below 0, so the raw predictions should not be used as customer risk scores. |
| Logistic regression | Test accuracy = 82.1% | Coefficients show how each feature changes the log-odds of churn, and exponentiated coefficients can be presented as odds ratios. The model also produces probabilities between 0 and 1. | Log-odds are less intuitive than percentage-point changes, and the current model assumes tenure has a linear effect on the log-odds scale. |
| GAM | Test ROC-AUC = 0.836, accuracy = 79.6%, precision = 0.651, recall = 0.503, F1 = 0.567, and Brier score = 0.140 | Smooth-effect plots show how churn risk changes across tenure and charge levels without forcing straight-line relationships. The model also gives valid probabilities and a direct calibration check. | The smooth curves are harder to summarize than one coefficient, and the 0.5 threshold identifies only about half of the customers who churned. |

## Recommendation

**Recommended model:** Logistic regression

**Why this model:** Logistic regression had the highest reported test accuracy at 82.1% and is a better fit for a binary outcome than linear regression. It also gives valid probabilities and stays easier to explain than the GAM. The GAM is still useful as a second model because it shows nonlinear patterns that logistic regression can miss, especially for tenure and monthly charges.

**What the company can responsibly conclude:** The models can be used to identify patterns associated with churn and to rank customers for further review. Longer contracts, greater tenure, and access to support-related services are associated with lower churn in these analyses, while fiber-optic service, electronic-check payment, and month-to-month contracts are associated with higher churn.

**What the company should not conclude yet:** These results do not show that changing one customer attribute will cause churn to increase or decrease. The models were tested on one historical dataset, and the reported performance values are not a perfectly controlled comparison because the notebooks used different feature sets and slightly different split procedures.

**One next analysis we would run:** Refit all three models using the same cleaned rows, predictors, stratified train/test split, and evaluation metrics. We would compare ROC-AUC, recall, calibration, and accuracy together, then tune the classification threshold based on the cost of missing a customer who is likely to churn.
