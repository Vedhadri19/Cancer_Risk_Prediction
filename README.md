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
