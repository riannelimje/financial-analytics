# Lab10

## Overview
Basically to evaluate creditworthiness based on binning techniques, grouping data, calculating WOE and IV 

## Dataset Information
The HMEQ dataset includes financial attributes of loan applicants. Key variables include:
- **BAD**: Loan status (1 = defaulted, 0 = paid on time)
- **LOAN**: Requested loan amount
- **MORTDUE**: Outstanding mortgage amount
- **CLAGE**: Age of oldest trade line in months
- **DEBTINC**: Debt-to-income ratio

## Techniques Used
### 1. Binning Methods
- **`pd.qcut()`**: Creates bins with equal frequencies.
- **`pd.cut()`**: Calculates equal sized bins or allows bin widths to be specified.
- **`df.groupby()`**: Splits the data into groups based on column values and computes summary statistics.

#### Example Code:
```python
woe_bin_data = hmeq_data.loc[:, ('CLAGE', 'BAD')]
woe_bin_data['Bin_Range'] = pd.qcut(hmeq_data['CLAGE'], q=10)
```

```python
bin_edges = [0, 50, 100, 150, 200, 250, 300, 10000]
hmeq_data_working['Bin_Range'] = pd.cut(hmeq_data['CLAGE'], bins=bin_edges, include_lowest=True)
```

### 2. Data Visualization
- **Seaborn Bar and Line Plots**:
    - `sns.barplot()`: Used to visualize categorical relationships.
    - `sns.lineplot()`: Used to overlay trend lines on bar plots.

#### Example Code:
```python
sns.barplot(x='Bin_Range', y='BAD', data=woe_bin_data)
sns.lineplot(x='Bin_Range', y='BAD', data=woe_bin_data, color='red')
```
### 3. Weight of Evidence (WOE) and Information Value (IV)
- **WOE**: Measures the predictive power of a variable by comparing the distribution of events (e.g., defaults) and non-events (e.g., non-defaults) across bins.
- **IV**: Summarizes the predictive power of a variable by aggregating the WOE across all bins.

$$
WOE = \ln\left(\frac{\% \text{Non-Events}}{\% \text{Events}}\right)
$$

#### IV Formula:
$$
IV_{\text{bin}} = (\% \text{Non-Events} - \% \text{Events}) \times WOE
$$
$$
IV_{\text{characteristic}} = \sum IV_{\text{bin}}
$$

#### Example Calculation:
For the range `<80` in `CLAGE`:
- **% Non-Events**: $$ \frac{152}{4623} \approx 3.3\% $$
- **% Events**: $$ \frac{162}{876} \approx 18.5\% $$
- **WOE**: $$ \ln\left(\frac{0.033}{0.185}\right) \approx -1.73 $$
- **IV for Bin**: $$ (0.033 - 0.185) \times -1.73 \approx 0.263 $$

### Example Workflow:

#### 1. Data Preparation:
- Clean and preprocess the dataset.
- Apply binning to variables like `CLAGE` (age of oldest trade line).

#### 2. WOE and IV Calculation:
- Calculate WOE and IV for each bin to evaluate the predictive power of variables.

#### 3. Visualization:
- Use bar and line plots to visualize trends in default rates across bins.

#### 4. Insights:
- Identify variables with high predictive power based on IV values.
- Highlight ranges (bins) with a high likelihood of default (e.g., WOE values).

## Learning Outcomes
- Understand how to apply **binning techniques** to credit risk data.
- Use **grouping functions** to derive insights from financial datasets.
- Visualize **loan default risk** trends effectively.
