# IS453 Session 09 Lab - Logistic Regression Model

This lab focuses on building and evaluating a logistic regression model for classification tasks. The notebook demonstrates how to preprocess data, train a model, and evaluate its performance using accuracy as the metric. Additionally, it introduces key concepts and functions related to logistic regression.

---

## Learning Objectives
- Understand the relationship between correlation and mechanistic connection.
- Identify explainable trends or shapes in data.
- Learn how to use logistic regression for classification tasks.
- Gain familiarity with new Python functions and methods for data preprocessing and model evaluation.

---

## Key Concepts

### 1. **Correlation vs. Mechanistic Connection**
   - Correlation indicates a statistical relationship between variables.
   - A mechanistic connection explains the underlying cause of the observed trend or shape.

### 2. **Explainable Trend or Shape**
   - Logistic regression models are used to identify trends or patterns in data that can be explained by independent variables.

---

## New Functions Introduced

### **Data Preprocessing**
- `df.dropna()`: Drops rows with missing values based on a specified threshold.

### **Logistic Regression (from `sklearn.linear_model`)**
- `LogisticRegression()`: Generates a logistic regression model.
- `logreg.fit(X, y)`: Trains (fits) the logistic regression model to the independent (`X`) and dependent (`y`) variables.
- `logreg.predict_proba()`: Returns the probability estimates for each class.
- `logreg.predict()`: Predicts class labels (e.g., 0 or 1) based on probability.

### **Model Evaluation**
- `sklearn.metrics.accuracy_score()`: Calculates the accuracy of model predictions.

---

## Logistic Regression Methods and Parameters

### **Key Methods**
- `lr = LogisticRegression()`: Initializes the logistic regression model.
- `lr.fit(X, y)`: Fits the model to the data.
- `lr.intercept_`: Retrieves the model's intercept.
- `lr.coef_`: Retrieves the model's coefficients.
- `lr.predict()`: Predicts class labels.
- `lr.predict_proba()`: Predicts probabilities for each class.

### **Important Parameters**
- `solver='liblinear'`: Specifies the optimization algorithm for logistic regression.
- `class_weight='balanced'`: Adjusts weights for imbalanced datasets.

---

## Code Examples

### **Data Preparation**
```python
# Select independent and dependent variables
X = hmeq_data[['LOAN']]
y = hmeq_data['BAD']

# Drop rows with missing values
hmeq_data = hmeq_data.dropna()
```

### **Model Training**
```python
# Initialize and train the logistic regression model
logreg = LogisticRegression(solver='liblinear', class_weight='balanced')
logreg.fit(X, y)
```

### **Model Evaluation**
```python 
# Predict class labels
y_pred = logreg.predict(X)

# Convert actual y values to array for comparison with predicted values
y_array = y.to_numpy()

# Compute accuracy
correct_predictions = sum(y_pred[i] == y_array[i] for i in range(len(y)))
accuracy = correct_predictions / len(y)
print(f'Model accuracy: {accuracy:.4}')
```

### **Logistic Regression Insights**
```python 
# Retrieve model intercept and coefficients
intercept = logreg.intercept_
coefficients = logreg.coef_

# Predict probabilities and class labels
logreg_prob = logreg.predict_proba(data_df)[0][1]
logreg_pred_status = logreg.predict(data_df)
```
### Additional Notes
#### How to Determine Predictive Variables
- Use correlation analysis to identify variables that have a strong relationship with the target variable.
- Examine the coefficients of the logistic regression model (logreg.coef_) to understand the impact of each variable on the prediction.
#### How to Create a Predictive Model
- Preprocess the data by cleaning it (e.g., using `df.dropna()`).
- Split the data into independent (X) and dependent (y) variables.
- Train a logistic regression model using `logreg.fit(X, y)`.
- Use the trained model to predict new values with `logreg.predict()` or `logreg.predict_proba()`.
#### How to Determine Model Accuracy
- Compare the predicted values (y_pred) with the actual values (y) using metrics like accuracy.
- Calculate accuracy as:
``` python 
accuracy = correct_predictions / len(y)
```
- Alternatively, use `sklearn.metrics.accuracy_score(y, y_pred)` for a concise calculation.