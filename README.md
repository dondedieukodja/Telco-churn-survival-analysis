# Telco-churn-survival-analysis
Customer churn prediction using survival analysis techniques

This project applies **survival analysis** to the Telco Customer Churn dataset in order to understand **how long customers remain active before churn** and which factors increase or reduce the risk of churn over time.

Unlike a standard churn classification project, this analysis focuses not only on *whether* a customer churns, but also on *when* the churn event is likely to occur. This makes the project especially relevant for retention, customer lifetime analysis, user activity monitoring, and product analytics.

The project was designed as a targeted portfolio project for a **Data Analyst / Data Scientist internship or apprenticeship**, with a specific link to use cases such as user identity lifecycle, inactive profile detection, retention monitoring, and business reporting.

---

## Business Problem

A company needs to better understand customer retention dynamics:

* When are customers most likely to churn?
* Which customer segments have the shortest lifetime?
* Which factors increase the risk of churn?
* Which factors protect customers from churn?
* How can the results support product, marketing, and business teams?

The goal is to transform raw customer data into actionable insights that can guide retention strategies and risk-based prioritization.

---

## Dataset

The project uses the public **Telco Customer Churn** dataset from Kaggle:

https://www.kaggle.com/datasets/blastchar/telco-customer-churn

The dataset contains customer-level information such as:

* customer tenure;
* contract type;
* internet service type;
* payment method;
* monthly charges;
* total charges;
* customer demographics;
* churn status.

In the survival analysis framework:

| Survival analysis concept | Dataset variable                                   |
| ------------------------- | -------------------------------------------------- |
| Individual observed       | Customer                                           |
| Duration                  | `tenure`                                           |
| Event                     | `Churn`                                            |
| `event = 1`               | Customer churned                                   |
| `event = 0`               | Customer still active / right-censored observation |

---

## Methodology

The project follows a complete data analysis workflow:

1. **Data import**
   Download of the dataset using `kagglehub` and loading of the CSV file into a pandas DataFrame.

2. **Data understanding**
   Inspection of dataset shape, variables, data types, missing values, and descriptive statistics.

3. **Data cleaning**
   Conversion of `TotalCharges` from text to numeric format, treatment of hidden missing values, and consistency checks.

4. **Survival analysis variable creation**
   Creation of `duration` from `tenure` and `event` from `Churn`.

5. **Exploratory business analysis**
   Analysis of churn by contract type, internet service, payment method, senior status, and customer characteristics.

6. **Kaplan-Meier survival analysis**
   Estimation of the global customer survival curve and survival curves by segment.

7. **Log-Rank statistical tests**
   Statistical comparison of survival curves across customer segments.

8. **Cox Proportional Hazards model**
   Estimation of hazard ratios to identify risk and protective factors.

9. **Model validation**
   Evaluation using the concordance index and discussion of model assumptions.

10. **Business recommendations**
    Translation of statistical findings into actionable retention strategies.

---

## Key Results

### Global churn and tenure

* Global churn rate: **26.5%**
* Median tenure: **29 months**
* Average tenure: **32.4 months**
* Maximum observed tenure: **72 months**

### Kaplan-Meier survival probabilities

| Time horizon | Probability of still being active |
| -----------: | --------------------------------: |
|     6 months |                             88.5% |
|    12 months |                             84.3% |
|    24 months |                             78.9% |
|    36 months |                             74.9% |
|    48 months |                             70.9% |

The Kaplan-Meier curve shows that churn risk is particularly important during the first months of the customer relationship. After approximately 24 months, the survival curve becomes more stable, suggesting that long-tenure customers tend to be more loyal.

The median survival time is not reached within the observation window, because the estimated survival probability does not fall below 50%.

---

## Cox Model Results

The Cox Proportional Hazards model achieved a concordance index of approximately:

> **C-index = 0.9029**

This indicates a strong ability to rank customers according to their relative churn risk.

### Main factors increasing churn risk

| Variable                         | Hazard Ratio | Interpretation    |
| -------------------------------- | -----------: | ----------------- |
| `InternetService_Fiber optic`    |         1.60 | Higher churn risk |
| `PaymentMethod_Electronic check` |         1.47 | Higher churn risk |
| `PaymentMethod_Mailed check`     |         1.26 | Higher churn risk |
| `PaperlessBilling_Yes`           |         1.20 | Higher churn risk |

### Main factors reducing churn risk

| Variable             | Hazard Ratio | Interpretation              |
| -------------------- | -----------: | --------------------------- |
| `Contract_Two year`  |         0.35 | Strongly reduces churn risk |
| `Contract_One year`  |         0.51 | Reduces churn risk          |
| `OnlineSecurity_Yes` |         0.65 | Reduces churn risk          |
| `TechSupport_Yes`    |         0.71 | Reduces churn risk          |
| `Partner_Yes`        |         0.73 | Reduces churn risk          |
| `OnlineBackup_Yes`   |         0.73 | Reduces churn risk          |

---

## Customer Risk Profiles

The Cox model was also used to estimate predicted survival curves for three typical customer profiles:

| Customer profile  | Probability of still being active after 12 months |
| ----------------- | ------------------------------------------------: |
| Low-risk profile  |                                             91.7% |
| Average profile   |                                             71.1% |
| High-risk profile |                                             43.1% |

This illustrates how survival analysis can be used to score customers individually and prioritize retention actions.

---

## Business Insights

The main insights from the project are:

1. Churn risk is strongest during the first year of the customer relationship.
2. Contract type is one of the most important drivers of retention.
3. Month-to-month contracts are much riskier than one-year or two-year contracts.
4. Electronic check payment is associated with higher churn risk.
5. Customers with fiber optic service show a higher churn risk, which may indicate pricing issues, unmet expectations, or service dissatisfaction.
6. Services such as online security, online backup, and tech support are associated with better retention.

---

## Business Recommendations

Based on the results, the company could:

* focus retention actions on customers in their first months;
* encourage customers to move from monthly contracts to annual contracts;
* monitor customers using electronic check payment more closely;
* improve the value proposition of fiber optic services;
* promote support and security services to increase customer engagement;
* build dashboards to track churn risk by customer segment.

---

## Link With First-id Use Cases

Although this project uses a telecom dataset, the methodology is transferable to identity and user lifecycle problems.

| Telco churn project  | First-id use case                             |
| -------------------- | --------------------------------------------- |
| Customer             | User identifier / digital profile             |
| Churn                | Inactivity or disappearance of an identifier  |
| Tenure               | Age of an identifier                          |
| Customer segment     | Identity segment or user group                |
| Survival probability | Probability that an identifier remains active |
| Cox hazard ratio     | Effect of an attribute on inactivity risk     |
| Retention dashboard  | Product / business monitoring dashboard       |

Potential applications for First-id:

* estimating the lifetime of a user identifier;
* detecting profiles at risk of inactivity;
* monitoring retention by identity source or segment;
* creating survival-based KPIs for product dashboards;
* prioritizing product or business actions based on risk scores.

---

## Tools and Libraries

* Python
* pandas
* numpy
* matplotlib
* seaborn
* lifelines
* kagglehub
* Jupyter Notebook

## Limitations

This project has several limitations:

* The dataset is static and does not capture changes in customer behavior over time.
* Some variables may violate the proportional hazards assumption.
* The results are based on a public dataset and should be interpreted as illustrative.
* More advanced versions could include time-varying covariates, stratified Cox models, or machine learning survival models.

---

## Possible Improvements

Future improvements could include:

* adding a retention dashboard in Looker Studio or Power BI;
* testing survival machine learning models;
* adding time-varying customer behavior;
* deploying an automated churn-risk scoring pipeline;
* comparing survival analysis with classical churn classification models;
* creating a Streamlit app to explore survival curves interactively.

---

## Author

**Don-de-Dieu KODJA**
Master 1 / Magistère in Econometrics, Statistics and Data Science
Interested in Data Analysis, Data Science, Product Analytics 
 
