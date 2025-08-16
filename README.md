# Portfolio Optimization Model

This project implements a dynamic portfolio allocation strategy using forward-looking economic indicators to suggest weights for stocks, bonds, and T-bills. The model uses historical data to train a regression model that maps expected GDP growth, inflation, interest rates, and equity valuations to optimal asset allocations based on risk-adjusted returns (e.g., Sharpe ratio).

The approach is divided into five key steps.

## Step 1: Define the 4 Inputs (Forward-Looking Indicators)

- **Expected GDP Growth**  
  Source: IMF World Economic Outlook, OECD forecasts, Bloomberg consensus surveys, or Fed Greenbook (historical).  
  Proxy if needed: PMI indices (Purchasing Managers’ Index) often anticipate GDP trends.

- **Expected Inflation**  
  Source: TIPS breakeven inflation rates (10y, 5y), University of Michigan Inflation Survey, SPF survey.

- **Expected Interest Rates**  
  Source: Fed Funds Futures (CME), Eurodollar futures, or forward Treasury yields (2y/10y forward curve).

- **Equity Valuations (Forward P/E or EPS Growth Expectations)**  
  Source: FactSet, Bloomberg, S&P Global, or public aggregates of forward P/E for major indices (e.g., S&P 500).

- TODO: Figure out what data is available and easily importable through python

## Step 2: Prepare the Training Data

### Features
- Expected GDP growth rate
- Expected inflation rate
- Expected interest rates
- Forward P/E ratio

### Targets (y)
- Expected stock market return rate
- Expected bond market return rate

Note that there is no need to compute the expected treasury bill return since is known.

#### Example Process
- Look at the next 3 months of realized returns.

Now you have a dataset of the form:

| GDP_exp | Infl_exp | Rate_exp | PE_fwd | w_stocks | w_bonds |
|---------|----------|----------|--------|----------|---------|
| 2.5%    | 2.0%     | 1.0%     | 18     | 1.60%    | 0.30%   |
| 1.0%    | 3.0%     | 2.5%     | 22     | -0.35%   | 0.45%   |
| …       | …        | …        | …      | …        | …       |

## Step 3: Train a Regression Model

- **Model Choice**: Start simple with linear regression. Consider experimenting with Random Forest / XGBoost if needed.
- **Input**: `[GDP_exp, Infl_exp, Rate_exp, PE_fwd]`.
- **Output**: `[w_stocks, w_bonds]`.

## Step 4: Predict Portfolio Weights (Live Use)

- Collect current forward-looking indicators (today’s expectations).
- Feed them into the model.
- Model outputs expected returns.
- Use MPT to compute tangency portfolio

#### Example Output

| Indicator Input                                        | Model expected 3-month returns | 3-month risk-free return | Optimal Allocation from MPT         |
|--------------------------------------------------------|--------------------------------|--------------------------|-------------------------------------|
| GDP_exp=2.2%, Infl_exp=2.0%, Rate_exp=1.5%, PE_fwd=20x | Stocks: 2.5%; Bonds: 1.5%      | 0.5%                     | 55% Stocks, 35% Bonds, 10% T-bills  |

ASSUMPTIONS:
- Treasury bills never default and will not be sold early
- Asset variances and covariances are constant (Consider using a second ML model to remove this assumption)
