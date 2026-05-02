# Weather Prediction: RainTomorrow Classifier

> **"Which model best handles the complexity of Australian weather prediction — and why does it win?"**

A multi-model machine learning comparison for predicting next-day rainfall using Australian weather data. Built with KNIME Analytics Platform for UTS 31250 Introduction to Data Analytics (Spring 2024).

---

## The Problem

Rain prediction matters. For agriculture, logistics, and disaster management, knowing whether it will rain tomorrow affects real decisions. This project asks: given 20+ environmental attributes about today's weather, which model most reliably predicts whether it will rain tomorrow?

---

## Dataset

**Target:** `RainTomorrow` — binary classification (Yes / No)

**Features (20+ attributes):**
MinTemp · MaxTemp · Rainfall · Evaporation · Sunshine · WindGustSpeed · WindDir9am · WindDir3pm · WindSpeed · Humidity · Pressure · Cloud · Temp · RainToday

**Missing data challenge:**

| Attribute | Missing % |
|---|---|
| Sunshine | 47.81% |
| Evaporation | 42.91% |
| Cloud9am | 37.9% |
| Cloud3pm | 40.33% |
| Pressure9am/3pm | ~9.9% |

Handling strategy: String attributes → Most Frequent Value; Numerical attributes → Median imputation.

---

## KNIME Workflow Pipeline

```
CSV Reader
  → Column Auto Type Cast
    → Category to Number
      → Missing Value (Median / Most Frequent)
        → Normalization (for SVM)
          → Partitioning
            → Model Training (6 models)
              → K-Cross Validation
                → Scorer / ROC
```

---

## Models Compared

| Model | Accuracy | AUC | Cohen's Kappa |
|---|---|---|---|
| Decision Tree | 82.03% | 0.8090 | 0.451 |
| K-Nearest Neighbour | 84.34% | 0.8543 | 0.472 |
| **Random Forest** | **84.59%** | **0.8615** | **0.466** |
| Support Vector Machine | 84.00% | 0.8615 | 0.461 |
| Neural Network | 84.41% | 0.8600 | 0.499 |
| Bagging Ensemble (DT) | — | — | — |

All models evaluated using K-Fold Cross Validation for robust, unbiased comparison.

---

## Best Classifier: Random Forest

**Why RF wins:**

- Highest accuracy (84.59%) across all models evaluated
- Strongest recall (0.964) — critical for minimizing missed rain events
- Built-in feature importance reveals which weather attributes drive predictions most
- Ensemble structure reduces overfitting risk vs. single Decision Tree
- Handles both categorical and numerical features without extensive preprocessing

**Kaggle Submission Score: 0.72473**

---

## Key Findings

- **Evaporation and Sunshine** are the most difficult features to handle due to high missingness (42–48%) — imputation strategy directly affects model stability
- **Humidity3pm** showed the strongest linear correlation with RainTomorrow
- Normalization was critical for SVM — without it, the model underperformed significantly
- The bagging ensemble (bootstrap sampling) showed that **smaller sample sizes improved accuracy**, highlighting the importance of parameter tuning even in ensemble methods
- Neural Network achieved the highest Cohen's Kappa (0.499) despite not having the top accuracy — suggesting it handles class imbalance slightly better

---

## Tech Stack

`KNIME Analytics Platform` · `K-Fold Cross Validation` · `Parameter Optimization` · `Correlation Matrix Analysis`

---

## Files

| File | Description |
|---|---|
| `weather_prediction.knwf` | KNIME workflow — full pipeline with all 6 models |
| `report.pdf` | Full project report with confusion matrices, ROC curves, and analysis |

---

## Context

Individual project. UTS Bachelor of IT — Data Analytics, Introduction to Data Analytics (Spring 2024).
