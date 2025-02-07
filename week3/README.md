# Lab 3

## Overview
This lab focuses on analysing and comparing fund performance against benchmarks using Python. Key topics include calculating annual returns, annualized returns, correlation of returns, and determining if funds are actively managed.

## Key Concepts

### 1. Reading and Storing Data
- **Store Data to CSV:**  Saves a DataFrame to a CSV file.
  ```python
  pandas.DataFrame.to_csv('filename.csv')
  ```

- **Read Data from CSV:**  Reads a CSV file into a DataFrame and sets an index column.
  ```python
  df = pandas.DataFrame.read_csv('filename.csv', index_col='MyInd')
  ```
  
### 2. Handling Missing Data
- **Check for Missing Values:**  Returns a DataFrame indicating missing values.
  ```python
  adj_close.isnull()
  ```
  
- **Count Missing Values:**  Sums the number of missing values per column.
  ```python
  adj_close.isnull().sum()
  ```
  
### 3. Return Calculations
- **Period Return:**  Computes total return over a given period.
  ```python
  period_return = (stocks_df_price.iloc[-1] - stocks_df_price.iloc[0]) / stocks_df_price.iloc[0]
  ```

- **Annualized Return:**  Converts total return into an annualized return over 3 years.
  ```python
  annualized_return_3y = ((1 + period_return_3y) ** (1/3)) - 1
  ```

- **Relative Annualized Return:**  Measures the difference in annualized return between two assets.
  ```python
  relative_annualized_ret = annualized_return_A - annualized_return_B
  ```

### 4. Correlation Analysis
- **Correlation Matrix:**  Calculates the correlation between all columns in a DataFrame.
  ```python
  pandas.DataFrame.corr()
  ```

- **Scatter Plot of Returns:**  Generates a scatter plot to visualize correlation.
  ```python
  plt.scatter(series_1, series_2)
  ```
  
  **Example:**  Plots the percentage returns of SPY vs. TLT.
  ```python
  plt.scatter(pct_return_df['SPY'], pct_return_df['TLT'])
  ```

### 5. Active vs Passive Management
- Actively managed funds tend to have lower correlation with their benchmark.
- A high correlation suggests passive management, closely tracking the benchmark.
- **Hint:** Active management will have lower correlation to the benchmark.

## Summary
- **Annual and annualized returns** help compare fund and benchmark performance.
- **Correlation analysis** reveals the relationship between funds and benchmarks.
- **Low correlation** suggests active management, while high correlation suggests passive management.
- **Handling missing data** is crucial for accurate analysis.
