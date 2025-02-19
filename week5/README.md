# IS453 Financial Analytics - Portfolio Risk & Return Analysis

## Overview
This lab focuses on quantifying the properties of return distributions using statistical techniques, calculating annualised returns, volatility, and Sharpe ratio for diversified portfolios and analysing the performance of a diversified portfolio.

## New Functions Used
- `plt.hist()`: Plots a histogram of a dataset.
- `norm.fit()`: Fits a dataset to a normal curve and returns its mean and standard deviation.
- `np.linspace()`: Returns an ordered array of equally distant numeric values.
- `norm.pdf()`: Returns a probability density function based on the normal distribution.
- `df.kurt()`: Returns excess kurtosis of values.
- `df.skew()`: Returns skewness of values.
- `sm.qqplot()`: Generates a QQ plot for a single dataframe column.

## Key Concepts & Formulas

### 1. Normal Distribution & PDF
- **Example Code**:
  ```python
  from scipy.stats import norm
  import numpy as np
  import matplotlib.pyplot as plt
  
  # Calculate the normal PDF
  mu, std = norm.fit(stocks_pct_chg['AAPL'])  
  xmin, xmax = plt.xlim() 
  x = np.linspace(xmin, xmax, 100) 
  p = norm.pdf(x, mu, std) 
  ```

### 2. Kurtosis
- **Example Code**:
  ```python
  stock_prices5y = data5y[['Adj Close']]
  stocks_pct_chg5y = stock_prices5y['Adj Close'].pct_change().dropna()
  
  print(f"5-year kurtosis: {stocks_pct_chg5y.kurt():.3}")
  ```

### 3. QQ Plot Analysis
- **Reading QQ Plots**:
  - **Leptokurtic**: Points deviate above the right end and/or below the left end of the line, indicating fat tails.
  - **Normal**: Points closely follow the line.
  - **Platykurtic**: Points deviate less from the center and tend to stay close to the line in the tails.
- **Example Code**:
  ```python
  import statsmodels.api as sm
  import matplotlib.pyplot as plt
  
  sm.qqplot(stocks_pct_chg['SPY'], line='s')
  plt.title('Normal Q-Q Plot SPY')
  plt.show()
  ```

### 4. Period Return & Annualized Return
- **Formula**:
  ```
  period return = (ending price - starting price) / starting price
  ARR = (1 + period return)^(1/N) - 1  # where N = number of years
  ```
- **Example Code**:
  ```python
  period_ret = (stock_prices2.iloc[-1] - stock_prices2.iloc[0]) / stock_prices2.iloc[0]
  
  annualized_returns = (1 + period_ret)**(1/2) -1
  print("Annualised returns for the 3 stocks:")
  print(annualized_returns.map(lambda x: f"{x:.2%}"))
  
  # This also works since the last value of the row in the cumulative return series is the period return 
  cum_ret_series = (1 + pct_returns).cumprod() - 1
  annualized_returns = (1 + cum_ret_series.iloc[-1])**(1/2) -1
  ```

### 5. Volatility & Annualized Volatility
- **Formula**:
  ```
  annualized_volatility = daily_volatility * sqrt(252)
  ```
- **Example Code**:
  ```python
  hv = np.log(stock_prices2 / stock_prices2.shift(1)).std()
  ann_volatilities = hv * np.sqrt(252)
  
  ann_volatility = portfolio_returns1.std() * np.sqrt(252)  # For percentage returns
  ```

### 6. Portfolio Weights & Returns
- **Example Code**:
  ```python
  # Define portfolio weights (corresponding to order of columns in returns df)
  # 'EFA', 'SPY', 'TLT'
  portfolio_weights = [0, 0.5, 0.5] 
  
  # Apply weights to returns
  wt_portfolio_ret = pct_returns * portfolio_weights
  ```

### 7. Sharpe Ratio Calculation
- **Formula**:
  $`S = \frac{R_p - R_f}{\sigma_p}`$

- **Example Code**:
  ```python
  rf_daily = 0.0001
  sharpe_ratio = (annualized_returns - rf_daily * 252) / ann_volatility
  ```
- **Interpreting Sharpe Ratio**:
  - **< 0**: Very bad
  - **0 - 1**: Not good
  - **1 - 1.99**: Acceptable to good
  - **2 - 2.99**: Very good
  - **> 3**: Great!

## Summary
- **Kurtosis & Skewness** help in understanding the shape of return distributions.
- **QQ Plots** aid in identifying the nature of the distribution (Leptokurtic, Normal, Platykurtic).
- **Annualized return & volatility** are essential for risk assessment.
- **Sharpe ratio** is a key metric for risk-adjusted performance evaluation.
- **Portfolio weighting** affects overall return and risk exposure.