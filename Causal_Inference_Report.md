# Deep Dive into Causal Inference: Insurance, Procedures, and Readmissions

---

# Abstract

This document explores the causal relationship between insurance status, hospital procedures, and hospital readmissions using a diabetes inpatient dataset. A critical re-evaluation of prior research is conducted using modern causal inference techniques, including the backdoor criterion, Directed Acyclic Graphs (DAGs), meta-learners (S-, T-, and X-learners), and sensitivity analysis. The investigation emphasizes distinguishing correlation from causation, understanding the roles of mediators and confounders, and interpreting conditional average treatment effects (CATEs). Reflections on methodology, limitations, and recommendations for future causal modeling practice are provided.

---

# Table of Contents

- [Introduction](#introduction)
- [Data Description](#data-description)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Readmission Rates Analysis](#readmission-rates-analysis)
- [Understanding Interaction vs. Causation](#understanding-interaction-vs-causation)
- [Causal Inference Analysis](#causal-inference-analysis)
  - [Insurance as a Confounder](#insurance-as-a-confounder)
  - [Insurance as Treatment Mediated through Procedures](#insurance-as-treatment-mediated-through-procedures)
  - [Complex DAG with Confounders, Mediators, and Direct Effects](#complex-dag-with-confounders-mediators-and-direct-effects)
- [How to Estimate the Causal Effect](#how-to-estimate-the-causal-effect)
- [Ordinary Regression vs. Causal Estimation](#ordinary-regression-vs-causal-estimation)
- [Modeling Approaches](#modeling-approaches)
  - [Logistic Regression](#logistic-regression)
  - [Random Forest, SVM, and XGBoost](#random-forest-svm-and-xgboost)
- [Causal Inference using S-, T-, and X-Learners](#causal-inference-using-s-t-and-x-learners)
- [DoWhy and Causal Graphs](#dowhy-and-causal-graphs)
- [Summary and Lessons Learned](#summary-and-lessons-learned)

---

# Introduction

This report explores a 2014 study that suggested HbA1c testing in diabetic inpatients is associated with lower readmission. While this claim is clinically intuitive, we revisit it through the lens of causal inference. Specifically, we ask: is HbA1c measurement itself causally protective, or merely a proxy for more comprehensive care influenced by insurance status?

By analyzing hospital data with over 70,000 records and applying causal inference techniques, we explore whether insurance status mediates or confounds the effect of treatment and how procedures performed during hospitalization relate to outcomes. This shift in framing—from predictive modeling to understanding causal mechanisms—is central to the insights presented.

---

# Data Description

The dataset spans 1999–2008 across 130 U.S. hospitals and includes over 100,000 diabetic patient encounters. After filtering for unique patients and one hospitalization per individual, the analysis uses ~71,000 rows.

Each record includes demographics, hospital visit characteristics, lab results (notably HbA1c), medications, and indicators of previous inpatient/outpatient/emergency visits.

HbA1c measurement was missing in ~78% of records, splitting the data naturally into:

- **Treatment group (HbA1c measured):** 15,388
- **Control group (not measured):** 56,130

Insurance status is inferred from the `payer_code` variable, where missing values are interpreted as uninsured.

---

# Exploratory Data Analysis

Initial summary:
- Length of hospital stay: 2–14 days
- Number of diagnoses: often >5, suggesting patients had serious conditions
- Readmission rates:
  - None: ~55K
  - <30 days: ~10K
  - >30 days: ~35K

Only the **number of procedures** showed imbalance between groups—higher in the treatment group—suggesting this variable could be a mediator of treatment effect.

---

# Readmission Rates Analysis

Key insights:

- Readmission rates are **higher** in the treatment group (HbA1c measured) than control, regardless of early or late readmission.
- When split by insurance status:
  - Among the **insured**, treatment group had **higher** readmission than control.
  - Among the **uninsured**, the opposite pattern appeared: treatment group had **lower** readmission.

This creates a **paradox**: is HbA1c measurement helpful or harmful? Or is insurance status interacting with or confounding this effect?

---

# Understanding Interaction vs. Causation

An interaction occurs when the effect of one variable (e.g., treatment) on the outcome (readmission) depends on another (e.g., insurance). In contrast, a causal effect means changes in one variable directly cause changes in another.

Regression with an interaction term helps detect modification but not necessarily causation. We use DAGs and causal frameworks to clarify this further.

---

# Causal Inference Analysis

## Insurance as a Confounder

DAG #1:

```
Insurance → Treatment (HbA1c)
Insurance → Readmission
Treatment → Readmission
```

If insurance status influences both treatment and readmission, it opens a **backdoor path**. To estimate the effect of HbA1c measurement on readmission, we must block this path—i.e., control for insurance status.

## Insurance as Treatment Mediated through Procedures

DAG #2:

```
Insurance → Procedures → Readmission
Insurance → Readmission
```

Here, procedures are **mediators**—adjusting for them blocks part of the total effect. If we control for procedures, we estimate the **direct effect** of insurance, not the total.

## Complex DAG with Confounders, Mediators, and Direct Effects

DAG #3:

```
C (Confounders) → Insurance → Procedures → Readmission
C (Confounders) → Readmission
Insurance → Readmission
```

In this structure, estimating the **total effect** of insurance requires controlling for confounders but **not procedures**.

---

# How to Estimate the Causal Effect

- **Total effect of insurance on readmission:** control for confounders, not mediators.
- **Direct effect of insurance:** control for both confounders and mediators.

---

# Ordinary Regression vs. Causal Estimation

Regression identifies associations. Causal estimation identifies causal paths using criteria like backdoor blocking. The distinction:

- Regression: control for covariates based on significance/correlation.
- Causal: control for covariates that **block backdoor paths** based on the DAG.

---

# Modeling Approaches

## Logistic Regression

Useful for interpretability. Predictors:
- Number of procedures
- Number of emergency visits
- Number of medications
- Insurance (Yes/No)
- HbA1c measured (Yes/No)
- Interaction: Insurance × HbA1c

## Random Forest, SVM, and XGBoost

Used for predictive power, but less interpretable. SHAP values can assist, though this study favors interpretability over raw accuracy.

---

# Causal Inference using S-, T-, and X-Learners

### S-Learner
Train one model using treatment indicator as a feature.
- CATE = \( \hat{Y}_{X=1} - \hat{Y}_{X=0} \)

### T-Learner
Train separate models for treatment and control groups.
- CATE = Treated prediction - Untreated prediction

### X-Learner
Combines both and adds modeling of imputed counterfactuals. Stronger when treatment/control groups are imbalanced.

#### Example:
- S: 15% (insured) – 20% (uninsured) = **-5%**
- T: 12% – 18% = **-6%**
- X: 11% – 17% = **-6%**

Interpretation: Insurance reduces this patient’s readmission risk by 5–6%.

---

# DoWhy and Causal Graphs

Used to formalize DAGs and validate assumptions:
- Visualizing dependencies
- Identifying colliders and mediators
- Validating backdoor paths

---

# Summary and Lessons Learned

This report documents both the analysis and thought process behind causal inference decisions. Key takeaways:

- Adjust only for **confounders** to estimate total effects
- Use **DAGs** early—nothing clarifies structure better
- Do **not** adjust for mediators unless estimating direct effects
- ML ≠ causal thinking: model interpretability matters

A checklist to guide future projects:
- [x] Identify treatment and outcome
- [x] Define total vs. direct effect
- [x] Draw DAG
- [x] Identify confounders, mediators, colliders
- [x] Choose appropriate learners
- [x] Estimate and interpret CATEs
- [x] Check for overlap, sensitivity, and assumptions

---

This write-up combines domain insight, causal frameworks, and model-based evaluation to clarify the often-misunderstood links between insurance, treatment, and hospital readmissions.

