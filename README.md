# Portfolio Optimization Model

This project implements a dynamic portfolio allocation strategy using forward-looking economic indicators to predict optimal asset allocations based on professional forecasts. The model uses historical data to train a regression model that maps expected GDP growth, inflation, interest rates, and equity risk premiums to optimal asset allocations based on risk-adjusted returns (e.g., Sharpe ratio).

The project provides a clear way for investors to pick their bond/stock allocation as a function of market outlook. Investors can adjust the cash weight based on their risk tolerance.

## Data Sources & Forward-Looking Indicators

The model uses four key forward-looking economic indicators from professional, institutional-grade data sources:

### 1. Expected GDP Growth
- **Source**: Philadelphia Federal Reserve Survey of Professional Forecasters (SPF)
- **Indicator**: DRGDP3 (Real GDP growth forecasts, 3 quarters ahead)
- **Description**: Quarterly surveys of professional economists from academia, government, and private sector
- **Coverage**: Longest continuous survey of macroeconomic forecasts (since 1968)
- **Data Format**: Inflation-adjusted GDP growth projections

### 2. Expected Inflation
- **Source**: Philadelphia Federal Reserve Survey of Professional Forecasters (SPF)
- **Indicator**: INFPGDP1YR (1-year GDP deflator inflation forecasts)
- **Description**: Professional economists' inflation expectations using GDP deflator
- **Advantage**: Uses GDP deflator which captures broader price changes than CPI
- **Fallback Strategy**: If data unavailable for target quarter, uses next available quarter

### 3. Expected Interest Rates
- **Source**: Philadelphia Federal Reserve Survey of Professional Forecasters (SPF)
- **Indicator**: TBILL1 (3-month Treasury bill rate forecasts)
- **Description**: Professional forecasters' expectations for short-term interest rates
- **Coverage**: Quarterly forecasts from professional economists
- **Backup**: TBILL2 (alternative T-bill forecast) if primary data unavailable

### 4. Equity Risk Premium
- **Source**: Damodaran Historical Risk Premium Data (NYU Stern)
- **Indicator**: Implied ERP (FCFE) - Free Cash Flow to Equity model
- **Description**: Historical equity risk premium derived from market data
- **Author**: Aswath Damodaran (adamodar@stern.nyu.edu)
- **Coverage**: Annual data from 1960-2024 with implied risk premiums

## Model Implementation

### Data Collection & Feature Engineering

All forward-looking indicators are implemented using professional forecaster data:

```python
# Example usage for Q4 2023 predictions:
gdp_growth = get_expected_gdp_growth(datetime.datetime(2023, 12, 31))      # 1.08%
inflation = get_expected_inflation(datetime.datetime(2023, 12, 31))        # 2.26%
interest_rate = get_expected_interest_rate(datetime.datetime(2023, 12, 31)) # 5.29%
risk_premium = get_risk_premium(datetime.datetime(2023, 12, 31))           # 4.60%
```

### Prepare the Training Data

#### Features (X)
- Expected GDP growth rate (from SPF DRGDP3)
- Expected inflation rate (from SPF INFPGDP1YR)
- Expected interest rates (from SPF TBILL1)
- Equity risk premium (from Damodaran Implied ERP FCFE)

#### Targets (y)
- Expected stock market return rate
- Expected bond market return rate

Note: Treasury bill returns are known from interest rate forecasts.

#### Training Data Structure
Look at the next 3 months of realized returns to create training dataset:

| GDP_exp | Infl_exp | Rate_exp | Risk_Prem | Stock_Return | Bond_Return |
|---------|----------|----------|-----------|--------------|-------------|
| 1.08%   | 2.26%    | 5.29%    | 4.60%     | TBD          | TBD         |
| 1.49%   | 3.05%    | 1.08%    | 5.94%     | TBD          | TBD         |
| 1.94%   | 2.19%    | 5.23%    | 4.33%     | TBD          | TBD         |

### Train a Regression Model

- **Model Choice**: Start with linear regression, consider Random Forest/XGBoost
- **Input**: `[GDP_exp, Infl_exp, Rate_exp, Risk_Prem]`
- **Output**: `[expected_stock_return, expected_bond_return]`

### Portfolio Optimization using Modern Portfolio Theory

- Use model predictions for expected returns
- Apply MPT to compute optimal tangency portfolio
- Consider transaction costs and constraints

### Predict Portfolio Weights (Live Use)

#### Example Workflow:
1. Collect current forward-looking indicators (today's expectations)
2. Feed them into trained model
3. Model outputs expected returns
4. Use MPT to compute optimal allocation

#### Example Output

| Forward-Looking Indicators                           | Model Expected Returns    | Risk-Free Rate | Optimal Allocation          |
|------------------------------------------------------|---------------------------|----------------|-----------------------------|
| GDP: 1.08%, Inflation: 2.26%, Rate: 5.29%, RP: 4.60% | Stocks: 2.5%, Bonds: 1.5% | 5.29%          | 55% Stocks, 35% Bonds, 10% Cash |


## Key Assumptions

- Treasury bills never default and will not be sold early
- Asset variances and covariances are estimated from historical data
- Professional forecasters provide unbiased expectations
- Market efficiency allows for translation of economic indicators to asset returns