# Portfolio Optimization with Forward-Looking Indicators

Welcome to the interactive portfolio optimization model! This project uses machine learning to predict stock and bond returns based on economic indicators.

## 🚀 Quick Start

Click the rocket icon (🚀) at the top of any notebook page to launch an interactive version where you can:
- Modify the code
- Run your own experiments  
- Test different economic scenarios
- Download your modified notebook

## 📚 What You'll Learn

1. **Data Collection**: Using Philadelphia Fed Survey of Professional Forecasters and Damodaran financial data
2. **Feature Engineering**: Converting economic indicators into model inputs
3. **Neural Network Training**: Building a PyTorch model to predict quarterly returns
4. **Model Validation**: Testing performance on out-of-sample data
5. **Portfolio Optimization**: Using Modern Portfolio Theory with predicted returns

## 🔧 Key Features

- **Self-contained model**: Just call `model.predict(gdp, inflation, rate, premium)` 
- **Automatic training**: Model trains itself when first used
- **Built-in validation**: `model.validate()` provides performance metrics
- **Real historical data**: 40+ years of quarterly data (1982-2023)
- **Professional data sources**: Philadelphia Fed forecasts + Damodaran returns

## 📊 Model Performance

The trained model achieves:
- **Stock predictions**: R² = 67.1%, RMSE = 2.21% (quarterly)  
- **Bond predictions**: R² = 63.3%, RMSE = 1.41% (quarterly)
- **Training period**: 1982-2023 (168 quarters)

## 🎯 Usage Example

```python
# Create and use the model
model = PortfolioPredictor()

# Make a prediction (auto-trains if needed)
stock_return, bond_return = model.predict(
    gdp_growth=0.025,    # 2.5% expected GDP growth
    inflation=0.03,      # 3% expected inflation  
    interest_rate=0.02,  # 2% expected interest rate
    risk_premium=0.05    # 5% equity risk premium
)

# Validate model performance
results = model.validate()
```

## 📈 Get Started

Click on "Portfolio Optimization Model" below to start exploring the interactive notebook!
