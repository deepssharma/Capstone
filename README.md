# Modeling Stock Prices and Portfolios Prediction - Capstone Project
--------------------------------------------------------------------
## Business Problem
**We would like to invest some money into stock market and have a portfolio that will maximize returns with as little risk as possible. We therefore want to minimize the risk involved while maximizing the profit.**
## Dataset Information
---------------------------------
The data was obtained from Yahoo Stock Finance using the python inbuilt library *yfinance*. We looked at the top 29 companies by weight in the S&P index (https://www.slickcharts.com/sp500), and this study includes modeling those 29 stocks and predicts portfolios with them. The symbols and abbreviations can be found at the above link (https://www.slickcharts.com/sp500).
  * The chosen order of symbols in the list is based on their weights in S&P index as described in the same link.
  * We chose a period of 10 years to look at the historical data. Also this period is used so that all the listed companies have data for the selected period.
## Analysis Approach
---------------------------------
* This is a multi-step problem that can be divided as follows:
    * First step us to build models that can predict the stock prices. The idea is that even though the model can not get the stock prices right, it should be able to predict the general trend of ups and downs in the stocks movement. Ideally we should use a few models to find the once that describes the trend best. But for now We are going to use only the below listed models:
        * Stacked LSTM model
        * ARIMA/GARCH models: done here only as exploratory analysis and will be included later as  keep improving the code quality.
    * Once we have a reliable model, we will generate predictions and then calculate returns on stocks and eventually build profitable portifolios.
    * We will use **Shapre Ratio** and also check **Volatility** as the measures to predict portfolios.
* <span style="color:lightblue"> One needs to include sentinet analysis as well to understand the effect of news and other factors on stocks price movement. However, at this point, we have not included it due to lack of time. </span>.
* <span style="color:lightblue"> Other thing to include is the information contained in the SEC filings of the companies and incorporate that into models.</span>
## Data Preparation
---------------------------------
* The data contains "Open", "Close, "Low", "High", "Adjusted Close" and "Volume" entries for each stock. Those values refer to the :
    * Open: stock opening price at the begining of the given day
    * Close: stock closing price at the end of the given day
    * Low: stock lowest price reached on the given day
    * High :Stock highest price reached on the given day
    * Adjusted Close: stock closing price on the given day after adjusting for various factors such as dividends distribution, applicable splits etc etc
    * Volume: Number of shares traded 
* Checked for stationarity in the time series by looking at auto-correlations, qq-plots, and adfuller test with and without transformation techniques (this is important to know if one builds ARIMA/GARCH models)
* Added several **Financial Indicators** to the data by using the python Librray **Finta**. Details can be found at: (https://pypi.org/project/finta/)
* The data was split into train, test and validation datasets. Since this is a time-series data, we can not shuffle data randomly. We used Keras TimeSeries Generator to build the data samples that preserve the time-order information
## Stocks Prediction using LSTM Model
---------------------------
(https://github.com/MohammadFneish7/Keras_LSTM_Diagram)
* We will use the **Long Short-Term Memory (LSTM) model**, a common deep learning recurrent neural network (RNN) used in predicting time series data. The diagram credit goes to (https://blog.floydhub.com/long-short-term-memory-from-zero-to-hero-with-pytorch/)

<img src="./lstm_diagram.png" alt="LSTM" class="bg-primary" width="500px" height="400px">

* LSTM has logic gates (input, output and forget gates) which give inherent ability for it to retain information that is more relevant and forgo unnecessary information. This makes LSTM a good model for interpreting patterns over long periods.
* The important thing to note about LSTM is the input, which needs to be in the form of a 3D vector (samples, time-steps, features). Hence, the input has to be reshaped to fit this.
## Risk-Adjusted Returns: (Shapre Ratio and Volatility)
------------------------------
The **Sharpe ratio**—also known as the modified Sharpe ratio or the Sharpe index—is a way to measure the performance of an investment by taking risk into account. 
The Sharpe Ratio is calculated by determining an asset or a portfolio’s “excess return” for a given period of time. This amount is divided by the portfolio’s standard deviation, which is a measure of its volatility. To calculate the Sharpe Ratio, use this formula:
  * Sharpe Ratio = (Rp – Rf) / Standard deviation where:
    * **Rp**: return of portfolio/mean return,
    * **Rf**: risk-free rate of 3.7% ie current risk free rate of US market,
    * **Standard Deviation**: standard deviation of the portfolio’s excess return,

Sharpe ratio **> 1** is considered **good**,**> 2** is considered **very good**, and **> 3** is considered **excellent**
### An Example of Model fitted to GOOGLE stocks Data and Prediction
<p float="left">
  <img src="https://github.com/deepssharma/Capstone/blob/master/plots/GOOG.png" width="500px" height="300px" />
  <img src="https://github.com/deepssharma/Capstone/blob/master/figures/future_GOOG.png" width="500px" height="300px" /> 
</p>

## Top 3 Portfolios:
--------------------------------------
Below one can see all the stocks that can be paired to each other using Shapre Ratio as the measure of profit
### Pairable Stocks Based on Shapre Ratio

<img src="https://github.com/deepssharma/Capstone/blob/master/figures/pairable_stocks.png" alt="pairable stocks" class="bg-primary" width="800px" height="400px">

Using Shapre Ratio and Portfolio Voltality, this analysis yields the following three best portfolios:
<img src="https://github.com/deepssharma/Capstone/blob/master/figures/top3_portfolios.png">

## Conclusions
---------------------------
* We have built a basic framework that:
    * fits LSTM models on all the stocks that are included in study.
    * builds Returns for each individual stocks.
    * predicts combinations of stocks to build profitable portfolios based on Shapre Ratio and Voltality.
## Future Improvements
---------------------------
* Check other models such as GARCH, Random Forests etc to get prediction for stocks movement.
* Hyperparameter tuning of Models.
* Explore the models by including few variables versus all the variables that are included in this study.
* Include the sentienet analysis which includes web-scrapping for news that affect stocks market.
* Also implement the information from SEC reports submitted by companies.

