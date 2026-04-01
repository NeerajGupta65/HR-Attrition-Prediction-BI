# HR Attrition Prediction & BI Dashboard

A end-to-end machine learning and business intelligence project that predicts employee attrition risk using the IBM HR Analytics dataset and visualizes insights through an interactive Power BI dashboard.

---

## Overview

Employee attrition is a costly challenge for organizations. This project combines predictive ML modeling with an interactive BI dashboard to help HR teams:
- Identify employees at high risk of leaving
- Understand the key drivers behind attrition
- Take proactive, data-driven retention decisions

---

## Project Structure

```
HR-Attrition-Prediction-BI/
│
├── HR_Attrition.ipynb          # ML pipeline: EDA → Feature Engineering → Modeling
├── HR_Analytics.pbix           # Power BI dashboard
├── final_attrition_dashboard.csv  # Model output exported to Power BI
└── README.md
```

---

## Machine Learning Pipeline

### Dataset
- **Source:** [IBM HR Analytics Attrition Dataset](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset) via KaggleHub
- **Size:** 1,470 employee records × 35 features

### Key Steps

1. **Exploratory Data Analysis**
   - Null checks, descriptive stats, correlation matrix
   - Heatmap visualization to identify feature relationships

2. **Feature Engineering**
   - Label encoding of categorical variables
   - Dropped zero-variance columns (`EmployeeCount`, `Over18`, `StandardHours`)
   - Selected 9 most impactful features:
     `OverTime`, `MonthlyIncome`, `JobLevel`, `TotalWorkingYears`, `YearsAtCompany`, `JobSatisfaction`, `EnvironmentSatisfaction`, `WorkLifeBalance`, `DistanceFromHome`

3. **Class Imbalance Handling**
   - Applied **SMOTE** (Synthetic Minority Over-sampling Technique) to balance the training set

4. **Modeling**
   - **Random Forest** with `GridSearchCV` (5-fold CV, F1-weighted scoring) — tuned hyperparameters: `n_estimators`, `max_depth`, `min_samples_split`, `min_samples_leaf`
   - **XGBoost** with `scale_pos_weight=3` for imbalance handling
   - **Soft Voting Ensemble** combining tuned Random Forest + Logistic Regression (with StandardScaler pipeline)

5. **Threshold Optimization**
   - Swept thresholds from 0.00 → 1.00 in 0.01 steps
   - Selected optimal threshold (**0.53**) based on maximum F1-Score on the test set

6. **Risk Scoring**
   - Generated a continuous **RiskScore** (0–100) per employee using final model probabilities
   - Categorized into: `Low` (< 30), `Medium` (30–70), `High` (> 70)
   - Exported to CSV for Power BI consumption

---

## Power BI Dashboard

The `HR_Analytics.pbix` dashboard ingests the model output and provides:
- Attrition rate breakdown by Department, Job Level, and Overtime status
- Risk category distribution across the workforce
- Income vs. Attrition analysis
- Employee-level risk scoring table for HR action

---

## Tech Stack

| Category | Tools |
|---|---|
| Language | Python 3 |
| ML Libraries | scikit-learn, XGBoost, imbalanced-learn |
| Data | pandas, NumPy, KaggleHub |
| Visualization | Matplotlib, Seaborn |
| BI Dashboard | Power BI |

---

## Results

- Threshold-optimized classification report at `t = 0.53`
- Risk scores exported per employee for actionable HR insights
- Ensemble model (RF + LR Voting) evaluated against standalone RF and XGBoost baselines

---

## Getting Started

```bash
# Clone the repo
git clone https://github.com/NeerajGupta65/HR-Attrition-Prediction-BI.git
cd HR-Attrition-Prediction-BI

# Install dependencies
pip install pandas numpy scikit-learn xgboost imbalanced-learn matplotlib seaborn kagglehub

# Run the notebook
jupyter notebook HR_Attrition.ipynb
```

Open `HR_Analytics.pbix` in **Power BI Desktop** to explore the dashboard.

---

## Dataset Source

IBM HR Analytics Employee Attrition & Performance — available on [Kaggle](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset).
