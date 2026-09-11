# Cancer Risk Level Prediction

End-to-end machine learning project that predicts a patient's **cancer risk level** (`Low` / `Medium` / `High`) from lifestyle, clinical, and environmental risk factors.

The final model is a **class-weighted XGBoost** classifier, hyperparameter-tuned with **Optuna**, and deployed via a **Streamlit** web app (batch CSV upload + single-patient manual input).

---

## Problem

Cancer risk is influenced by many factors (smoking, BMI, family history, air pollution, etc.). This project builds a multiclass classifier that estimates overall risk level so that high-risk individuals can be flagged for follow-up.

**Important:** This is an educational / research prototype, **not** a medical device. Predictions must not be used for clinical decisions.

---

## Dataset

File: `cancer-risk-factors.csv`

| Item | Value |
|------|--------|
| Samples | 2,000 patients |
| Features used | 17 (see below) |
| Target | `Risk_Level` ∈ {Low, Medium, High} |
| Cancer types present | Lung, Breast, Colon, Prostate, Skin |

**Class distribution (imbalanced):**

| Risk Level | Count |
|------------|-------|
| Medium     | 1,574 |
| Low        | 324   |
| High       | 102   |

**Features used by the model:**
Age, Gender, Smoking, Alcohol_Use, Obesity, Family_History,
Diet_Red_Meat, Diet_Salted_Processed, Fruit_Veg_Intake,
Physical_Activity, Air_Pollution, Occupational_Hazards,
BRCA_Mutation, H_Pylori_Infection, Calcium_Intake,
BMI, Physical_Activity_Level


**Excluded columns (and why):**
- `Patient_ID` — identifier only
- `Cancer_Type` — outcome-like grouping; including it would let the model “cheat”
- `Overall_Risk_Score` — derived score that leaks target information

---

## Approach

1. **Baseline models** — Random Forest, Logistic Regression, Decision Tree
2. **Imbalance handling** — class weights (inverse frequency) and experiments with SMOTE
3. **Final model** — XGBoost with sample weights + Optuna hyperparameter search (macro-F1 / High-class recall focus)
4. **Persistence** — model, label encoder, and feature list saved as `.pkl`
5. **Deployment** — Streamlit app for interactive and batch prediction

### Final model highlights

- Algorithm: XGBoost (`multi:softmax`)
- Handling of imbalance: class weights
- Tuning: Optuna (TPE sampler)
- Output: risk class + class probabilities

Example metrics from a class-weighted run (test set, n=400):

| Class  | Precision | Recall | F1-score |
|--------|-----------|--------|----------|
| High   | 0.21      | 0.75   | 0.33     |
| Low    | 0.42      | 0.82   | 0.56     |
| Medium | 0.92      | 0.59   | 0.72     |
| **Macro avg** | 0.52 | **0.72** | 0.54 |

*(High-class recall was deliberately prioritised so fewer high-risk patients are missed.)*

---

### Future improvements

Collect a larger / more balanced real-world dataset
Add calibration of probability estimates
Explore cost-sensitive learning or threshold tuning for High class
Deploy as a REST API (FastAPI) in addition to Streamlit
Feature importance / SHAP explanations in the UI
