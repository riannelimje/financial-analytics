# Lab7

## Overview
The HMEQ dataset contains information on 5,960 home equity loans, including loan characteristics and delinquency status. The data helps in analyzing factors that contribute to loan default.

## Key Variables
- **BAD**: Loan status (1 = defaulted, 0 = paid/current)
- **LOAN**: Requested loan amount
- **MORTDUE**: Amount due on mortgage
- **VALUE**: Property value
- **REASON**: Loan purpose (DebtCon/HomeImp)
- **JOB**: Occupational category (ProfExe, Mgr, Office, Self, Sales, Other)
- **YOJ**: Years at current job
- **DEROG**: Number of major derogatory reports
- **DELINQ**: Number of delinquent credit lines
- **CLAGE**: Age of oldest credit line (months)
- **NINQ**: Number of recent credit inquiries
- **CLNO**: Number of credit lines
- **DEBTINC**: Debt-to-income ratio (%)
---

- **sns.boxplot()**: Creates box plots to visualize data distribution and outliers.
- **sns.countplot()**: Creates bar charts for categorical variables.
- **sns.kdeplot()**: Generates KDE plots for understanding distributions.
- **df.groupby()**: Aggregates data for comparative analysis.
- **df.unstack()**: Converts pivot table structures into columns for better visualization.

---
## Univariate Analysis
Univariate analysis examines a single variable at a time to understand its distribution and key statistics.
## Bivariate Analysis
Bivariate analysis examines relationships between two variables.

## Functions Used in Analysis
### 1. **Boxplots**
#### Purpose:
Used to visualise the distribution of numerical variables and detect outliers.
#### Example Code:
```python
large_val_numeric_var = ['LOAN', 'MORTDUE', 'VALUE', 'YOJ', 'CLAGE', 'DEBTINC', 'CLNO']
fig, axes = plt.subplots(ncols=2, nrows=int(np.ceil(len(large_val_numeric_var)/2)), figsize=(32, 64))
for i, axis in enumerate(fig.axes):
    sns.boxplot(ax=axis, data=hmeq_data[large_val_numeric_var[i]], orient='h')
    axis.set_xlabel(str(large_val_numeric_var[i]))
    if i == len(large_val_numeric_var)-1:
        break
plt.show()
```

### 2. **Countplots** (Bar Charts for Categorical Variables)
#### Purpose:
Used to visualise categorical distributions and frequencies.
#### Example Code:
```python
sns.countplot(x = hmeq_data['BAD'])
plt.title('Good count vs Bad count')
plt.show()
```

### 3. **KDE Plots**
#### Purpose:
Kernel Density Estimation (KDE) plots help visualise the distribution of numerical variables and their relationship to loan default status.
#### Example Code:
```python
plt.figure(figsize=(9,5))
sns.kdeplot(data=hmeq_data, x="LOAN", hue="BAD", fill=True)
plt.title('Distribution of Loan amount by Default Rate')
plt.show()
```

### 4. **Groupby() and Unstack()** (Comparing Relative Percentages)
#### Purpose:
Used to compare distributions of categorical variables against the target variable (`BAD`).
#### Example Code:
```python
cat_low_val_num_var = ['JOB', 'REASON', 'DEROG', 'DELINQ', 'NINQ']
fig, axes = plt.subplots(ncols=2, nrows=int(np.ceil(len(cat_low_val_num_var)/2)), figsize=(32, 32))
for i, axis in enumerate(fig.axes):
    df_bivariate = hmeq_data.groupby(cat_low_val_num_var[i])['BAD'].value_counts(normalize=True).unstack()
    df_bivariate.plot(ax=axis, kind='bar', stacked=True)
    axis.set_xlabel(str(cat_low_val_num_var[i]))
    axis.legend(bbox_to_anchor=(1.0, 1.0))
    if i == len(cat_low_val_num_var)-1:
        break
plt.show()
```

### 5. **Correlation Matrix & Heatmap**
#### Purpose:
Used to analyze relationships between numerical variables.
#### Example Code:
```python
cor = hmeq_data.corr()
fig, ax = plt.subplots(figsize=(10,8))
sns.heatmap(cor, xticklabels=cor.columns, yticklabels=cor.columns, annot=True, cmap="YlGnBu", ax=ax)
plt.show()
```

---

## Concepts to Remember
- **Boxplots** help detect outliers.
- **Bar charts** visualize categorical distributions.
- **KDE plots** help analyze distributions by category.
- **Groupby() and Unstack()** allow comparisons of default rates across categories.
- **Correlation matrices** help us better understand which and how much variables are related.
- **Bar charts and KDE plots** help us better conceptualize univariate and bivariate distributions
