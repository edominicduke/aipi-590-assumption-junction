# Team Name: AEJ

## Contributors

- Alberto — Linear Regression

- Ethan — Logistic Regression

- Jaideep — GAM

## Dataset

The Telco Customer Churn dataset (IBM sample data) has 7,043 customers from a telecom company, one row per customer. Each row has account info (contract type, payment method, tenure), billing (monthly/total charges), which services the customer has (phone, internet, streaming, security add-ons), and a `Churn` label saying whether that customer left the company. The task is to predict `Churn` from these attributes so the company can identify at-risk customers before they leave.

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| Linear regression | Multicollinearity, linearity, independence of observations, homoscedasticity, normality of residuals, influential observations | Multicollinearity: found and fixed two issues (redundant "no service" categories and MonthlyCharges/TotalCharges overlapping with other features), final VIF < 3 for every feature. Linearity: churn rate showed a reasonably linear downward relationship with tenure. Independence: reasonable because each row represents a different customer, although unobserved clustering cannot be fully ruled out. Homoscedasticity: Breusch-Pagan p ≈ 0. Normality: Jarque-Bera p ≈ 0. Influence: max Cook's Distance = 0.0021; although some observations exceeded the 4/n screening threshold, no single observation clearly dominated the fit. | Homoscedasticity and normality both fail. These violations are expected with a binary target and limit the reliability of standard OLS inference, particularly conventional standard errors, p-values, and confidence intervals |
| Logistic regression | Binary outcome, Independence of observations, No perfect separation, Linear log-odds, Limited multicollinearity, and Sufficient sample size | Binary outcome: The target (Churn) only has two values (0 and 1); Independence of observations: No customers appear more than once and no completely duplicated observations were found; No perfect separation: No category has observations from only one outcome class as determined by pd.crosstab; Linear log-odds: The plot of the log-odds of churn vs tenure (the only continuous variable) is a diagonal line (demonstrating linearity in the log-odds of churn with respect to tenure); Limited multicollinearity: The highly correlated features TotalCharges and MonthlyCharges were removed (see notebook for more information) | No concerns given the passing of all of the assumptions |
| GAM | Binary outcome, independent observations, coverage across predictors, category support, and possible concurvity | After cleaning, 7,032 unique customers remained and the churn rate was 26.6%. Each factor level had support, and the train/test split was stratified. The strongest continuous-predictor correlation was 0.889. Smoothing penalties were selected using the training data only. | Tenure and TotalCharges are strongly related, so their smooth effects may divide the same signal. The model is additive and may miss interactions. Results should be treated as associations, not causal effects. |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| Linear regression | R² = 0.26, RMSE = 0.38 on the test set; 79.8% accuracy and 50.5% churn recall when predictions are thresholded at 0.5 | Coefficients are very easy to explain: each one is a direct change in churn probability, in percentage points, no extra math needed for a non-technical audience | About 17% of predictions fall outside the valid [0, 1] probability range (down to about -24%), so the raw outputs are not valid probabilities and are unsuitable as calibrated risk scores |
| Logistic regression | Accuracy on Test Set = 82.11% | Coefficients can easily explain how each feature impacts the log-odds of churn (a  a one unit increase in SeniorCitizen results in a 0.16312020439311975 increase in the log-odds of churn) | Might be difficult for a non-technical audience to understand what it means for a variable to affect the "log-odds" of another variable (in this case churn) |
| GAM | Test ROC-AUC = 0.836, accuracy = 79.6%, precision = 0.651, recall = 0.503, F1 = 0.567, and Brier score = 0.140 | Smooth-effect plots show how churn risk changes across tenure and charge levels without forcing straight-line relationships. The model also gives valid probabilities and a direct calibration check. |The smooth curves are harder to summarize than one coefficient, and the 0.5 threshold identifies only about half of the customers who churned. |

## Recommendation

**Recommended model:** Logistic regression

**Why this model:** Logistic regression had the highest reported test accuracy at 82.1% and is a better fit for a binary outcome than linear regression. It also gives valid probabilities and stays easier to explain than the GAM. The GAM is still useful as a second model because it shows nonlinear patterns that logistic regression can miss, especially for tenure and monthly charges.

**What the company can responsibly conclude:** The models can be used to identify patterns associated with churn and to rank customers for further review. Longer contracts, greater tenure, and access to support-related services are associated with lower churn in these analyses, while fiber-optic service, electronic-check payment, and month-to-month contracts are associated with higher churn.

**What the company should not conclude yet:** These results do not show that changing one customer attribute will cause churn to increase or decrease. The models were tested on one historical dataset, and the reported performance values are not a perfectly controlled comparison because the notebooks used different feature sets and slightly different split procedures.

**One next analysis we would run:** Refit all three models using the same cleaned rows, predictors, stratified train/test split, and evaluation metrics. We would compare ROC-AUC, recall, calibration, and accuracy together, then tune the classification threshold based on the cost of missing a customer who is likely to churn.