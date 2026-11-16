# Dynamic and Explainable Credit Risk Early-Warning System with Portfolio Stress Testing

## Project Overview

This project develops a forward-looking, explainable credit risk early-warning framework to identify currently performing mortgage loans that may experience 30+ days of delinquency within the next six months.

The framework integrates borrower-level risk prediction, feature explainability, probability calibration, dynamic risk monitoring, portfolio exposure analysis and illustrative stress sensitivity assessment.

## Objectives

- Develop a six-month forward-looking credit risk prediction framework.
- Engineer borrower, loan, financial and historical repayment behaviour features.
- Evaluate machine-learning models using chronological validation and out-of-time testing.
- Interpret model predictions using SHAP.
- Calibrate predicted risk probabilities.
- Track borrower risk movements over time.
- Aggregate borrower-level predictions for portfolio analysis and stress sensitivity.

## Dataset

**Source:** Fannie Mae Single-Family Loan Performance Data  
**Acquisition cohort:** 2018 Q1

The processed source dataset contains:

- 23.56 million loan-month observations.
- 458,830 unique loans.
- 99 reporting months.
- Reporting period: January 2018 – March 2026.

The original dataset is not included in this repository. Refer to the official Fannie Mae data source and its usage terms for access.

## Target Variable

The target variable is `target_6m_early_warning`.

- **1:** A currently performing loan becomes 30+ days delinquent in at least one of the following six calendar months.
- **0:** The loan remains current throughout all six future calendar months.

The final target dataset contains approximately 20.16 million valid observations, with an event rate of approximately 3.21%.

## Methodology

1. Data ingestion and validation
2. Target construction and leakage prevention
3. Feature engineering
4. Chronological training, validation and test splitting
5. Model development and evaluation
6. SHAP-based explainability
7. Probability calibration
8. Dynamic borrower-level risk monitoring
9. Portfolio concentration analysis
10. Illustrative stress sensitivity analysis

## Feature Engineering

The model uses multiple categories of features:

- **Borrower and loan:** Credit score, DTI, LTV, original loan amount and loan term.
- **Financial dynamics:** Current outstanding balance, interest rate and loan maturity.
- **Repayment behaviour:** Historical delinquency indicators over 3, 6, 12 and 24 months.
- **Temporal:** Loan age, reporting year and reporting month.
- **Categorical:** Loan purpose, property type, occupancy status and property state.

Future delinquency information is used exclusively for target construction, not as a predictor.

## Models

- Regularised Logistic Regression (SGDClassifier with log-loss)
- Histogram Gradient Boosting

Chronological validation and out-of-time testing are used to preserve the temporal structure of the problem.

## Test Results

| Metric | Logistic Regression | Histogram Gradient Boosting |
|---|---:|---:|
| ROC-AUC | 0.5039 | 0.8289 |
| PR-AUC | 0.0312 | 0.2892 |
| Brier Score (raw) | 0.9562 | 0.09435 |

The HGB model achieved substantially stronger ranking performance than the baseline model.

After sigmoid calibration, the HGB test Brier score improved to approximately 0.02513.

## Explainability and Early Warning

SHAP was used to investigate feature contributions to model predictions.

A validation-selected raw-score threshold of 0.58 was used for borrower warning classification. The corresponding validation recall was approximately 49.92%, with precision of 22.62%.

The dynamic monitoring framework tracks newly flagged, persistently flagged, de-escalated and below-threshold borrower states.

## Portfolio Analysis and Stress Testing

The September 2025 portfolio snapshot contained:

- 111,141 loan accounts.
- Approximately US$16.98 billion outstanding UPB.
- Approximately US$1.247 billion flagged UPB.
- 6,759 flagged accounts.

An illustrative stress sensitivity analysis was conducted using different PD multipliers and LGD assumptions.

The baseline scenario with a 30% LGD assumption produced an estimated six-month loss sensitivity of approximately US$164.46 million.

**Note:** This is an illustrative sensitivity analysis, not a full IFRS 9 expected credit loss calculation or regulatory capital estimate.

## Technology Stack

- Python
- Pandas and NumPy
- DuckDB
- Scikit-learn
- SHAP
- Matplotlib
- Jupyter Notebook
- Streamlit
- Git and GitHub

## Repository Structure

```text
dynamic-credit-risk-early-warning/
├── notebooks/
├── figures/
├── reports/
├── src/
├── dashboard/
├── presentation/
├── results/
├── .gitignore
├── requirements.txt
└── README.md
```

## Limitations

- The analysis uses a specific mortgage acquisition cohort.
- The HGB model was trained using a training-period subsample for computational feasibility.
- The exploratory SHAP sample was limited and requires further class-aware validation.
- Probability calibration may require monitoring for temporal drift.
- Stress scenarios use assumed PD multipliers and LGD values.
- The framework is a research prototype and requires further validation before operational deployment.

## Author

**Mudavath Madhu**  
Dual Degree – Industrial and Systems Engineering  
Indian Institute of Technology Kharagpur

## Disclaimer

This project was developed for academic research. The findings are analytical estimates based on the selected dataset, modelling methodology and stated assumptions. They should not be interpreted as financial advice or production credit decisions.