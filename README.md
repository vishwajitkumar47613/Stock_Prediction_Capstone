# Predicting Stock Price using time-series data:- Capstone Project

<img width="1536" height="1024" alt="Project_chart" src="https://github.com/user-attachments/assets/f73ef61d-ed73-4e58-a464-074aaaa6fad9" />

## Business Understanding

  **Research Question:** Can we accurately predict the next seven days' closing price for a select group of top U.S. stocks using historical time-series data? 📈

  **Why This Question Matters**: Accurately forecasting stock prices could offer a significant advantage to both individual investors and large financial institutions. If this question remains unanswered, investors must rely on intuition, news, or basic analysis, which can lead to poor financial decisions. My analysis aims to provide a data-driven tool that can serve as a valuable piece of business intelligence. By providing a reliable forecast with clear confidence levels, my work can help investors make more informed decisions, manage risk more effectively, and potentially increase returns. This isn't just about crunching numbers; it's about translating complex data patterns into a clear, actionable insight that anyone can understand and use to improve their financial strategy.

  **Data Source:** I have used the yfinance Python library to pull five years of daily stock data, including opening and closing prices, adjusted closing prices, and trading volumes, directly from the trusted Yahoo Finance API.

  **Techniques**: For this project, i have used Linear Regression model as a baseline. To improve accuracy, I'll then apply more sophisticated machine learning techniques: ARIMA for its ability to model time-series trends, Random Forest to capture complex relationships, and finally, a deep learning model, Long Short-Term Memory (LSTM), to handle the intricate temporal dependencies in the data.

  **Expected Results**: Have a Linear Regression (LR) baseline model for forecasting the daily Adjusted Closing Price of four major US stocks (AAPL, GOOG, MSFT, TSLA).In the next project, more ML models will be applied to show clear improvement in accuracy from the simple linear model to the more advanced time-series and deep learning approach. 


## Summary of the Capstone Project part 1

In this project, i have established a **Linear Regression (LR) baseline model** for forecasting the daily Adjusted Closing Price of four **major US stocks (AAPL, GOOG, MSFT, TSLA).** The model uses five years of historical data and incorporates **macroeconomic indicators (VIX, SPY) and technical features (Moving Average, Volatility)** to predict the price for a 7-day forecast period.

The primary purpose of this baseline is to **provide a measurable benchmark**. We aim to improve upon these initial results using more **advanced models (Random Forest, ARIMA, LSTM)** in next phase of this project.

<img width="1108" height="415" alt="Screenshot 2025-09-29 at 3 52 27 PM" src="https://github.com/user-attachments/assets/47fb2c9a-f771-4da6-903e-6ef71f1042e7" />

## Exploratory Data Analysis (EDA) and Data Quality
This task was critcal as it performed Exploratory Data Analysis and Visualization of the data.It did pre-processing and cleaning of data and generate multiple plots to understand the relationship between different features.It helped working on cleaned, feature-enhanced data (that includes VIX and SPY) to visually inspect for uncovering relationships and verifying data quality before modeling begins.

### Data Acquisition and Storage
  - Used the yfinance Python library to fetch historical financial data.

  - Stock Tickers (AAPL, GOOG, MSFT, TSLA): Data was fetched for the Adjusted Close price, which corrects for corporate actions (splits, dividends) to provide the most accurate price history.

  - Macro Indicators (VIX, SPY): Macro data was fetched using the yfinance.Ticker().history() method.

  - Local Storage and Retrieval: To ensure efficient and repeatable execution, all downloaded stock data was saved locally as CSV files in the dedicated stock_data directory. The pipeline is configured to check for a local file first before attempting a re-download. If a local file is found to be corrupted or outdated, the code automatically deletes it and downloads a fresh copy.

### Data Inspection and Cleaning
  - Macro Integration: The VIX (Volatility Index) and SPY (S&P 500 ETF) were successfully merged using a Left Join followed by Backward/Forward Filling (bfill().ffill()) to handle date misalignments, ensuring a complete feature set without data loss.

  - Missing Values: All missing values resulting from merging and feature calculation (technical indicator look-back windows) were successfully imputed or dropped, resulting in clean, ready-to-model data.

### Plotting Price Trends and Returns Distribution

  - Visual Trend (Historical Closing Price): 
<img width="1400" height="500" alt="AAPL_price_and_returns" src="https://github.com/user-attachments/assets/405eb12f-12f2-452f-9f3d-a9f7782788c9" />
<img width="1400" height="500" alt="GOOG_price_and_returns" src="https://github.com/user-attachments/assets/da41d5a7-9fbd-45d1-a513-04e96a4aa109" />
<img width="1400" height="500" alt="MSFT_price_and_returns" src="https://github.com/user-attachments/assets/2b42ae10-9d50-49d8-9cf7-27d1bfeadc2b" />
<img width="1400" height="500" alt="TSLA_price_and_returns" src="https://github.com/user-attachments/assets/5c169251-aab5-4f0b-ae68-a306e9accae5" />

**Summary**
  - All stocks show a strong upward trend over the 5-year period, confirming the data is non-stationary (a requirement for differencing in ARIMA).
  - TSLA generally exhibits the highest overall volatility and largest magnitude of price changes compared to the generally steadier growth of MSFT.Overall, despite big fluctuations, TSLA remains in a broad long-term uptrend.
  - MSFT stock price shows a clear long-term upward trend from early 2021 to mid-2025.
  - GOOG stock price shows a significant upward trend over the five-year period.This reflects strong capital appreciation and a positive long-term outlook for the stock.The clear, long-term trend means the price series is non-stationary.
  - APPL stock price exhibits a clear upward trend over the observed period. This signifies substantial growth and strong positive performance over the long run.
    
**Returns Distribution:** The histograms show that daily returns are typically centered around zero for most stocks but exhibit fat tails, indicating that extreme positive or negative movements are more common than a simple normal distribution would suggest.

**TSLA Daily Returns Distribution**

  **Key observations**
  - Center at 0 → Most daily returns are very close to zero.
  - Bell-shaped distribution → Roughly normal
  - Fat tails → Both left and right tails are heavier than a normal distribution, meaning extreme moves happen more often than a normal distribution would suggest.
  - Width of the distribution: Most daily moves are within ±5%, but there are many days outside that, even up to ±15–20%.


**MSFT Daily Returns Distribution**
  
  **Key observations**
  - The distribution is bell-shaped (unimodal and symmetric), centered very close to zero. The returns mostly cluster between -0.05 (-5%) and 0.05 (5%). 
  - The distribution has fatter tails than a perfect normal distribution.
  - The fatter tails suggest that extreme daily movements (volatility/risk) occur more frequently than a standard normal distribution would predict.

**GOOG Daily Returns Distribution**

  **Key observations**
  - The distribution of daily returns is bell-shaped and symmetrical, with its mean clustered very close to zero. This suggests that on any given day, a price movement up or down is roughly equally likely, and the typical change is minimal.
  - The distribution has a higher peak at the center and heavier or "fatter tails" compared to a perfect normal distribution.
  - The fatter tails indicate that extreme daily price moves (large gains or losses) occur more frequently than a standard statistical model might predict.

**APPL Daily Returns Distribution**

  **Key observations**
  - The distribution is largely symmetrical and centered near zero, which is typical for liquid stock returns. Most days see only small price changes.
  - The histogram is peaked higher at the center and has fatter tails than a standard normal distribution.

### Feature Correlation Analysis

The correlation analysis helps validate the choice of external features. We analyzed correlation between the stock price and macro indicators, and between stock returns and macro returns.

**Stock Price vs. Macro Indicators (Correlation Plot):** 


<img width="800" height="600" alt="GOOG_price_macro_correlation" src="https://github.com/user-attachments/assets/24653cc8-1643-4b6a-a2ca-ec41b37697be" />

<img width="800" height="600" alt="AAPL_price_macro_correlation" src="https://github.com/user-attachments/assets/52c22737-cd1b-4087-b4a6-6f6839d06011" />

<img width="800" height="600" alt="MSFT_price_macro_correlation" src="https://github.com/user-attachments/assets/3dbaa433-dd1b-4ca4-a144-1ea2ed8dbf12" />
<img width="800" height="600" alt="TSLA_price_macro_correlation" src="https://github.com/user-attachments/assets/3cfc0901-f777-470a-91c8-cd873e358b73" />



  - Stock vs. SPY (ρ Price, SPY): We observe a strong positive correlation (typically ρ>0.8) for all tickers. This confirms that the stock's absolute price is highly influenced by the overall market trend, making SPY an essential feature.

  - Stock vs. VIX (ρ Price, VIX ): We observe a moderate negative correlation (typically ρ≈−0.4 to −0.6). As expected, when the VIX (volatilit index also known as fear index) rises, stock prices tend to drop.

**Stock Returns vs. Macro Changes:**

  - Stock Returns vs. SPY Returns:
  Correlation Matrix (Stock Returns vs Macro Changes) for APPL stock:
VIX_Change   -0.529390
SPY_Change    0.756528
    - This correlation remains strong, confirming that daily price movements are tightly linked to the market's daily movements. 
  
  - Stock Returns vs. VIX Change: This correlation is typically negative, confirming the inverse relationship: a large positive change in VIX signals fear and correlates with negative stock returns.


### Feature Engineering and Baseline Model Strategy

**Feature Engineering**

The model's performance depends mainly on transforming the raw time series data into sophisticated features using a 60-day look-back window (time_step=60).

  - **Technical Features:** Moving Average (MA), 60-day Returns, and Volatility were calculated, providing the model with measures of **trend, momentum, and risk**.
  - **Lagged Features:** All 6 features (Close, MA, Volatility, Returns, VIX, SPY) were lagged 60 times, generating a total of 6×60=360 **input features (X) per prediction**.
  - **Multi-Step Target:** The model was trained for 7 simultaneous outputs (y), forecasting the price for the entire next week at once.

**Linear Regression Baseline Performance**
The Linear Regression model serves as a simple, interpretable model to establish the starting point.

  - **Training Method:** The model was trained chronologically on 80% of the historical data.
  - **Evaluation Metrics:**
    - **RMSE:** Measures prediction error in absolute dollar terms.
    - **MAPE:** Measures prediction error as a percentage, providing an interpretable measure of forecast quality.
  - **Stocks Prediction Visualization**
  <img width="1600" height="800" alt="TSLA_lr_baseline_forecast" src="https://github.com/user-attachments/assets/5d892cc7-265b-466c-b278-1175e1cea3a9" />
  <img width="1600" height="800" alt="MSFT_lr_baseline_forecast" src="https://github.com/user-attachments/assets/799e62b2-b1f0-46cb-a06e-32249a77591a" />
  <img width="1600" height="800" alt="GOOG_lr_baseline_forecast" src="https://github.com/user-attachments/assets/4998e25d-59f1-4e39-b940-316e05cc6c2f" />
  <img width="1600" height="800" alt="AAPL_lr_baseline_forecast" src="https://github.com/user-attachments/assets/6fe43e19-da8a-4836-af28-f78d46555196" />

The visualization shows that the Linear Regression model generally does a pretty good job of capturing the overall trend of the actual price (the orange line) but struggles to capture sharp turns or localized volatility, resulting in a smoothed forecast (the dashed red lines).


### Conclusion and Next Steps
The Linear Regression baseline has successfully provided predictions with a low/moderate MAPE, confirming that the engineered features have significant predictive power. However, the model's performance is inherently limited by its assumption of linearity.
  - **Next Steps:** We anticipate that complex, non-linear models like Random Forest (to capture non-linear feature interactions),ARIMA(targeting the temporal/time-based dependencies inherent in stock prices) and LSTM (to leverage sequence memory) will significantly outperform this Linear Regression baseline by reducing the RMSE and MAPE figures.
