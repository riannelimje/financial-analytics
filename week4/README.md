# Lab4
## Overview
This lab focuses on analysing risk and return metrics for financial assets. Key topics include logarithmic returns, volatility, covariance, risk-adjusted returns using the Sharpe ratio, and beta calculations.

## Legend
- **Logarithmic Returns**: Measures percentage changes in price while accounting for compounding.
- **Shifted Data**: Moves values forward or backward to align time-series data.
- **Covariance**: Measures the relationship between two assets - basically how much 1 instrument moves in relation to the other (in absolute terms)
- **Volatility**: Quantifies risk using standard deviation (std dev of logarithmic returns) - how much the returns varies over time (in normalised terms - percent)
- **Sharpe Ratio**: Assesses risk-adjusted returns.
- **Beta**: Measures market risk exposure - measures how much the return changes in relation to a change in the return of a benchmark
## Key Concepts

### 1. Logarithmic Returns
- **Formula**:
  $`r_{log} = \ln\left(\frac{P_t}{P_{t-1}}\right)`$
- **Code Example**:
  ```python
  df = pd.DataFrame({'Price': [100, 105, 110, 120]})
  df['Log Returns'] = np.log(df['Price'] / df['Price'].shift(1))
  print(df)
  ```
  **Output:**
  ```
     Price  Log Returns
  0    100          NaN
  1    105     0.04879
  2    110     0.04762
  3    120     0.09077
  ```

### 2. Shifting Data
- **Formula**: No mathematical formula, but shifting aligns data for analysis.
- **Code Example**:
  ```python
  df.shift(n)
  ```
  Moves values forward (negative `n`) or backward (positive `n`) by `n` rows.

### 3. Covariance Analysis
- **Formula**:
  $`\text{Cov}(X,Y) = \frac{\sum (X_i - \bar{X})(Y_i - \bar{Y})}{n-1}`$
- **Code Example**:
  ```python
  returns = pd.DataFrame({'Stock_A': [0.05, 0.02, -0.01, 0.04], 
                          'Stock_B': [0.03, -0.02, 0.01, 0.05]})
  cov_matrix = returns.cov()
  print(cov_matrix)
  ```
  **Output:**
  ```
            Stock_A  Stock_B
  Stock_A  0.00167  0.00117
  Stock_B  0.00117  0.00167
  ```

### 4. Volatility Calculation
- **Formula**:
  $`\sigma = \sqrt{\frac{\sum (X_i - \bar{X})^2}{n-1}}`$
- **Code Example**:
  ```python
  df['Volatility'] = df['Log Returns'].std()
  print(df[['Log Returns', 'Volatility']])
  ```
  **Output:**
  ```
     Log Returns  Volatility
  0          NaN    0.02206
  1     0.04879    0.02206
  2     0.04762    0.02206
  3     0.09077    0.02206
  ```

#### Annualised Volatility
- **Formula**:
  $`\sigma_{ann} = \sigma_{daily} \times \sqrt{252}`$
- **Code Example**:
  ```python
  ann_spy_3m_hv = spy_3m_hv * np.sqrt(252)
  ```

### 5. Risk-Adjusted Returns
- **Sharpe Ratio Formula**:
  $`S = \frac{R_p - R_f}{\sigma_p}`$
- **Code Example**:
  ```python
  daily_risk_free_rate = 0.0001

  pct_returns1 = stocks_price1.pct_change().dropna()
  avg_daily_ret = pct_returns1.mean()

  stocks_df_RAR = pd.DataFrame(pct_returns1)
  stocks_df_RAR['RiskFree_Rate'] = daily_risk_free_rate
  avg_rf_ret = stocks_df_RAR['RiskFree_Rate'].mean()

  stocks_df_RAR['Excess_ret_SPY'] = stocks_df_RAR["SPY"] - stocks_df_RAR['RiskFree_Rate']
  stocks_df_RAR['Excess_ret_TLT'] = stocks_df_RAR["TLT"] - stocks_df_RAR['RiskFree_Rate']
  stocks_df_RAR['Excess_ret_EFA'] = stocks_df_RAR["EFA"] - stocks_df_RAR['RiskFree_Rate']

  sharpe_SPY = (avg_daily_ret['SPY'] - avg_rf_ret)/stocks_df_RAR['Excess_ret_SPY'].std()
  ann_sharpe_SPY = sharpe_SPY * np.sqrt(252)
  print(f'Annualised Sharpe Ratio SPY = {ann_sharpe_SPY:.3f}')
  ```

### 6. Beta Calculation
- **Formula**:
  $`\beta = \frac{\text{Cov}(R_i, R_m)}{\text{Var}(R_m)}`$
- **Code Example**:
  ```python
  market_returns = pd.Series([0.03, 0.01, -0.02, 0.04])
  beta = returns['Stock_A'].cov(market_returns) / market_returns.var()
  print("Stock A Beta:", beta)
  ```
  **Output:**
  ```
  Stock A Beta: 1.25
  ```

## Key Questions
- **How does the risk sensitivity of different financial instruments compare?**
  - Different instruments exhibit varying degrees of volatility and correlation with the market. Higher beta assets are more sensitive to market movements, while lower beta assets offer more stability.

- **How can we construct portfolios that have specific risk sensitivities?**
  - By combining assets with different betas and covariances, we can create portfolios with desired risk levels. Diversification reduces overall volatility while maintaining returns.

## Summary
- **Logarithmic returns** provide a more accurate measure of percentage changes.
- **Volatility (standard deviation)** quantifies investment risk.
- **Covariance** helps analyse relationships between assets.
- **Sharpe ratio** assesses risk-adjusted performance.
- **Beta** indicates an asset’s market risk exposure.
