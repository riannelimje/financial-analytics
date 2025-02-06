# Lab2

## Overview
This lab focuses on analysing financial time series data using Python. It explores stock prices over different time frames, compares adjusted and raw closing prices, and calculates cumulative returns for performance evaluation.

## Key Concepts
### Cumulative Returns Calculation
- **Daily Percentage Change:**   Computes the percentage change between one row and the next.
  ```python
  pandas.DataFrame.pct_change()
  ```  
- **Cumulative Product Calculation:**   Computes the cumulative product for each row.
  ```python
  pandas.DataFrame.cumprod()
  ```  
- **Cumulative Return Calculation:**
  ```python
  cum_ret_series = (stocks_df['Adj Close'].pct_change() + 1).cumprod() - 1
  ```
- **Visualizing Percentage Change:**
  ```python
  from matplotlib.ticker import FuncFormatter
  ax1 = pct_change.plot(figsize=(16,9), title='Daily Percentage Change in AAPL Prices')
  ax1.yaxis.set_major_formatter(FuncFormatter(lambda y, _: '{:.0%}'.format(y)))
  plt.show()
  ```
- **Formatting Cumulative Returns:**
  ```python
  '{:.1%}'.format(cum_ret_series.iloc[-1])  
  ```
  *Note:* `cum_return_series.iloc[-1].map('{:.1%}'.format)` raises an AttributeError since `map()` is for iterable objects like Series, whereas `iloc[-1]` retrieves a scalar (float). Use direct string formatting instead.
  
  **Example Usage of `map()`:**
  ```python
  cum_return_series.map('{:.1%}'.format)  # Works for entire Series
  ```
  - **Series (`map()` can be used):** A Pandas Series is a one-dimensional array-like structure. It supports element-wise operations using `.map()`, which applies a function or formatting operation to each element of the Series.
  - **Scalar (`map()` cannot be used):** When using `.iloc[-1]`, you are selecting a **single value** (scalar) from a Series. Since it is not an iterable (not a Series anymore), `.map()` will raise an `AttributeError`.
  
  ```python
  '{:.1%}'.format(cum_return_series.iloc[-1])  
  ```
  In this case, direct string formatting is used on the single float value extracted from the Series.
  
  **TLDR:**
  - Use **`.map()`** when applying formatting to an entire Pandas Series.
  - Use **direct formatting (`'{:.1%}'.format(value)`)** when working with a single scalar value extracted from a Series.


## Summary
- **Stock price charts reveal trends but are not ideal for comparisons.**
- **Adjusted Close is better for evaluating performance over time.**
- **Adjusted Close accounts for dividends and stock splits, making it more suitable for return calculations.**
- **Close only accounts for stock splits.**
- **Difference between Close and Adjusted Close increases over time.**
- **Cumulative return charts provide a clear view of long-term gains and losses, but not useful for specific discrete subperiods (eg. individual month/years) since it aggregates returns over the entire period.**

