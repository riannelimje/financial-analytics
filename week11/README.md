# Lab 11 

## Overview
This lab focuses on constructing a **credit risk scorecard** using the `scorecardpy` library. The scorecard is a predictive model used to assess creditworthiness by transforming raw data into a scoring system. The lab demonstrates the end-to-end process of building a scorecard, including binning, Weight of Evidence (WOE) transformation, and model evaluation.

---

## Key Objectives
1. Learn how to preprocess data for scorecard modeling.
2. Apply binning techniques and WOE transformation using `scorecardpy`.
3. Build and evaluate a logistic regression model for credit scoring.
4. Construct a credit risk scorecard from the logistic regression model.

---

## Techniques and Concepts Covered

### 1. **Binning and WOE Transformation**
- **Binning**: Group continuous variables into discrete intervals.
- **WOE Transformation**: Convert raw data into WOE values to improve model interpretability and performance.

#### Example Code:
```python
import scorecardpy as sc

# Perform binning
bins = sc.woebin(hmeq_data, y='BAD')

# Apply WOE transformation
hmeq_woe = sc.woebin_ply(hmeq_data, bins)
```

### 2. **Logistic Regression Model**
- Train a logistic regression model using WOE-transformed data.
- Evaluate the model's performance using metrics like AUC (Area Under the Curve).

#### Example Code:
```python
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import roc_auc_score

# Train logistic regression model
X = hmeq_woe.drop(columns=['BAD'])
y = hmeq_woe['BAD']
model = LogisticRegression().fit(X, y)

# Evaluate model performance
y_pred = model.predict_proba(X)[:, 1]
auc = roc_auc_score(y, y_pred)
print(f"AUC: {auc}")
```

### 3. **Scorecard Construction**
- Convert the logistic regression model into a scorecard using `scorecardpy`.
- Assign scores to variables and calculate total credit scores.

#### Example Code:
```python
# Create a scorecard
scorecard = sc.scorecard(bins, model, points0=600, odds0=1/20, pdo=50)

# Calculate credit scores
scores = sc.scorecard_ply(hmeq_data, scorecard)
print(scores.head())
```

---

## New Functions Used

### 1. `sc.woebin_ply()`
- **Description**: Performs Weight of Evidence (WOE) encoding on data using predefined bins.

### 2. `sc.scorecard()`
- **Description**: Generates a credit risk scorecard from logistic regression model coefficients and binning information.

### 3. `sc.scorecard_ply()`
- **Description**: Calculates credit scores for data based on the generated scorecard.

### 4. `sklearn.metrics.confusion_matrix()`
- **Description**: Computes the confusion matrix to evaluate the accuracy of a classification model.

### 5. `sklearn.metrics.classification_report()`
- **Description**: Generates a detailed report of precision, recall, F1-score, and support for each class.

---

## Tools and Libraries Used
- **scorecardpy**: For binning, WOE transformation, and scorecard construction.
- **sklearn**: For logistic regression modeling and evaluation.

## Learning Outcomes
- Understand the process of building a credit risk scorecard.
- Learn how to preprocess data using binning and WOE transformation.
- Train and evaluate a logistic regression model for credit scoring.
- Construct a scorecard and calculate credit scores for applicants.
