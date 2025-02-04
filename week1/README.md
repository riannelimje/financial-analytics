# Week 1

## Overview
This lab explores key concepts in data processing and analysis using Python. The exercises focus on loading datasets, performing data transformations, and analysing the results.

## Main Functions and Their Purpose

### **Basic DataFrame Operations**
- `df.head(n)`: Selects the first `n` rows (default is 5).
  - Example: `stocks_df.head()` returns the first 5 rows.
  - Example: `stocks_df.head(3)` selects the first 3 rows.
- `df.tail(n)`: Selects the last `n` rows (default is 5).
  - Example: `stocks_df.tail(2)` selects the last 2 rows.
- `df.shape`: Provides the dimensions of the DataFrame as `(rows, columns)`.
  - Example: `stocks_df.shape` might return `(252, 6)`, meaning 252 rows and 6 columns.
- `df.info()`: Displays information about columns, including data types and non-null values.
  - Example: `stocks_df.info()` provides metadata about stock data.
- `df.isna()`: Returns `True` for each element that is missing (NaN) and `False` otherwise.
  - Example: `stocks_df.isna().sum()` checks for missing values in each column.

### **Data Aggregation and Indexing**
- `df.sum()`: Calculates the sum of values for each column.
  - Example: `stocks_df['Volume'].sum()` returns the total trading volume.
- `df.index`: Returns the index labels of the DataFrame.
  - Example: `stocks_df.index` shows the list of dates if indexed by time.

### **Selecting Data**
- `df.iloc[rows, columns]`: Selects rows and/or columns by **position**.
  - Uses zero-based indexing.
  - Row slicing is exclusive on the end index (`start:end` means it includes `start` but excludes `end`).
  - Example:
    ```python
    stocks_df.iloc[0:3, 1:3]  # Selects rows 0 to 2 and columns 1 to 2 (this is slicing)
    stocks_df.iloc[[0, 3], [1, 2]]  # Selects specific rows 0 and 3, and columns 1 and 2 (this is [inclusive, inclusive])
    ```

- `df.loc[rows, columns]`: Selects rows and/or columns by **label**.
  - Row and column slicing is **inclusive**.
  - Example:
    ```python
    stocks_df.loc['2021-01-29':'2021-02-02', ['Close', 'Volume']]  # Selects all rows within the date range, and 'Close' & 'Volume' columns
    ```
  - Can also be used to filter data by conditions:
    ```python
    stocks_df.loc[stocks_df['Volume'] > 50000000, ['Close', 'Volume']]  # Selects rows where Volume > 50M
    ```

- `df[columns]`: Selects a column (or columns) by name, returning a Series if a single column is selected.
  - Example: `stocks_df['Close']` returns a Series.
- `df[[columns]]`: Selects one or more columns and **always returns a DataFrame**.
  - Example: `stocks_df[['Close']]` returns a DataFrame instead of a Series.

### **DataFrame Modifications**
- `df.copy()`: Creates a copy of the DataFrame to avoid modifying the original.
  - Example: `df_copy = stocks_df.copy()`.
- `df.rename(columns={'old_name': 'new_name'})`: Renames columns.
  - Example: `stocks_df.rename(columns={'Adj Close': 'Adjusted Price'})` renames the column `Adj Close` to `Adjusted Price`.
  - To rename the index: `stocks_df.rename(index={'2021-01-01': 'New Year'})`.

### **Plotting Data**
- `plt.plot(x, y)`: Plots data using Matplotlib.
  - Example:
    ```python
    import matplotlib.pyplot as plt
    plt.plot(stocks_df.index, stocks_df['Close'])  # Plots the closing prices over time
    plt.xlabel('Date')
    plt.ylabel('Closing Price')
    plt.title('Stock Closing Prices Over Time')
    plt.show()
    ```
### **Series vs Dataframes**
- **Series**: 1 dimensional, array like object that can hold a **single** column of data
  - only one data type for the whole array
  - does not have a header
  - using `col1 = stocks_df.iloc[ : , 4]`
  ```
  Date
  2020-12-31    129.751587
  2021-01-04    126.544205
  2021-01-05    128.108765
  2021-01-06    123.796432
  2021-01-07    128.020767
                   ...    
  2021-12-27    177.423676
  2021-12-28    176.400436
  2021-12-29    176.488968
  2021-12-30    175.328018
  2021-12-31    174.708160
  Name: Adj Close, Length: 253, dtype: float64
  ```
- **Dataframe**: 2 dimensional structure, composed of rows and columns
  - can have different data types for each column
  - comparing with the same dataset at the one above (it has a header for Adj close)
  - using `col1 = stocks_df.iloc[ : , [4]]`
  ```
              Adj Close
  Date	
  2020-12-31	129.751617
  2021-01-04	126.544243
  2021-01-05	128.108749
  2021-01-06	123.796440
  2021-01-07	128.020767
  ...	...
  2021-12-27	177.423630
  2021-12-28	176.400421
  2021-12-29	176.488998
  2021-12-30	175.328003
  2021-12-31	174.708145
  ```
  - multiple columns example
  ```
    	Open	High	Low	Close	Adj Close	Volume
  Date						
  2020-12-31	134.080002	134.740005	131.720001	132.690002	129.751587	99116600
  2021-01-04	133.520004	133.610001	126.760002	129.410004	126.544205	143301900
  2021-01-05	128.889999	131.740005	128.429993	131.009995	128.108765	97664900
  2021-01-06	127.720001	131.050003	126.379997	126.599998	123.796432	155088000
  2021-01-07	128.360001	131.630005	127.860001	130.919998	128.020767	109578200
  ```
