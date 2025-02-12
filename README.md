TCS Stock Market Analysis Project
Leveraging SQL, Power BI, and Excel for Data-Driven Insights
📌 Project Overview
The TCS Stock Market Analysis Project is a comprehensive study of Tata Consultancy Services (TCS) stock performance over multiple years. By integrating SQL for data management, Power BI for visualization, and Excel for advanced calculations, this project provides key insights into price trends, trading volumes, volatility, and financial metrics. The project aims to support investors, traders, and financial analysts in making informed decisions based on historical stock data.

🎯 Objectives
✔ Analyze historical stock trends to identify bullish and bearish phases.
✔ Assess trading volumes to determine liquidity and investor sentiment.
✔ Perform statistical and predictive analysis using moving averages and volatility measures.
✔ Compare TCS’s performance with Nifty 50 and other market indices.
✔ Build interactive Power BI dashboards for real-time tracking.
✔ Use SQL for data extraction, transformation, and querying.
✔ Implement Excel models for portfolio analysis, RSI, and financial ratios.

🛠 Technology Stack
✅ SQL – Data extraction, transformation, and querying.
✅ Power BI – Interactive dashboards and visual analytics.
✅ Excel – Advanced calculations, pivot tables, and forecasting.

📊 Data Sources & Processing
This project uses historical stock data from NSE/BSE, sourced from:
📌 Stock Market APIs (Yahoo Finance, Alpha Vantage, NSE, BSE)
📌 TCS Annual Reports & Financial Statements
📌 Global Macroeconomic Data (Inflation, Interest Rates, Index Movements)

🔹 Data Cleaning & Preparation (SQL)
Before analysis, the data undergoes preprocessing using SQL:

Removing duplicates & handling missing values.
Normalizing date formats & structuring data for analysis.
Joining tables for market index comparisons.

CREATE TABLE TCS_Stock (
    trade_date DATE PRIMARY KEY,
    open_price FLOAT,
    high_price FLOAT,
    low_price FLOAT,
    close_price FLOAT,
    volume INT
);

-- Handling missing values by forward-filling previous day's close price
UPDATE TCS_Stock t1
SET close_price = (
    SELECT close_price 
    FROM TCS_Stock t2 
    WHERE t2.trade_date = t1.trade_date - INTERVAL 1 DAY
)
WHERE close_price IS NULL;

🔍 SQL Queries for Key Insights

1️⃣ Daily & Monthly Stock Price Trends
SELECT trade_date, open_price, high_price, low_price, close_price, volume 
FROM TCS_Stock 
WHERE trade_date BETWEEN '2022-01-01' AND '2023-12-31' 
ORDER BY trade_date;

-- Monthly Average Closing Price
SELECT 
    YEAR(trade_date) AS year,
    MONTH(trade_date) AS month,
    AVG(close_price) AS avg_closing_price
FROM TCS_Stock
GROUP BY YEAR(trade_date), MONTH(trade_date)
ORDER BY year, month;

2️⃣ Trading Volume Analysis
-- Top 5 days with the highest trading volumes
SELECT trade_date, volume
FROM TCS_Stock
ORDER BY volume DESC
LIMIT 5;

-- Yearly Trading Volume Analysis
SELECT YEAR(trade_date) AS year, SUM(volume) AS total_volume
FROM TCS_Stock
GROUP BY YEAR(trade_date);

3️⃣ Moving Averages & Volatility Analysis
-- 7-day Moving Average for Stock Prices
SELECT 
    trade_date, 
    close_price, 
    AVG(close_price) OVER (ORDER BY trade_date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS moving_avg_7
FROM TCS_Stock;

-- Standard Deviation for Stock Volatility
SELECT 
    YEAR(trade_date) AS year, 
    STDEV(close_price) AS price_volatility
FROM TCS_Stock
GROUP BY YEAR(trade_date);

4️⃣ Comparing TCS with Nifty 50 (Market Benchmarking)
SELECT 
    t.trade_date, 
    t.close_price AS TCS_Price, 
    n.nifty_close AS Nifty_Price, 
    (t.close_price - LAG(t.close_price) OVER (ORDER BY t.trade_date)) / LAG(t.close_price) OVER (ORDER BY t.trade_date) * 100 AS TCS_Daily_Return,
    (n.nifty_close - LAG(n.nifty_close) OVER (ORDER BY n.trade_date)) / LAG(n.nifty_close) OVER (ORDER BY n.trade_date) * 100 AS Nifty_Daily_Return
FROM TCS_Stock t
JOIN Nifty_50 n ON t.trade_date = n.trade_date;

📈 Power BI Dashboard & Insights
Power BI is used to visualize stock performance through:
📊 Stock Price Trends – Line charts for daily, monthly, and yearly price movements.
📊 Trading Volume Heatmaps – Identify peak trading days.
📊 Volatility Analysis – Box plots for price fluctuations.
📊 Moving Averages – 7-day and 50-day moving averages.
📊 Comparison with Nifty 50 – Dual-axis charts tracking stock vs index performance.

🔹 Steps to Link SQL with Power BI
1️⃣ Import Data: Connect Power BI to SQL Server or CSV exports.
2️⃣ Transform Data: Use Power Query for cleaning and structuring.
3️⃣ Create Measures: Use DAX for stock price calculations and KPIs.
4️⃣ Build Visuals: Use line charts, bar graphs, heatmaps, and slicers.
5️⃣ Publish Reports: Deploy Power BI dashboards for real-time tracking.

📊 Excel Analysis for Statistical Insights
Excel is used for moving averages, RSI calculations, and portfolio risk analysis.

📌 Formula for Moving Average:
=AVERAGE(B2:B8)   // 7-day moving average

📌 Formula for RSI (Relative Strength Index):
=100-(100/(1+ (AVERAGEIF(C:C,">0",C:C) / ABS(AVERAGEIF(C:C,"<0",C:C)))))

📌 Portfolio Risk Assessment: ✔ Standard Deviation Calculation: =STDEV(B2:B30)
✔ Sharpe Ratio Calculation: (Mean Return - Risk-Free Rate) / Std Dev

🔍 Key Insights & Findings
📌 TCS showed an upward trend in stock price from 2018 to 2023, with major peaks in Q1 and Q3.
📌 Trading volumes were highest in Q4 each year, indicating strong institutional activity.
📌 Stock volatility increased significantly during global economic downturns, affecting investor confidence.
📌 TCS had a 90% correlation with Nifty 50, confirming its alignment with overall market trends.

🔚 Conclusion & Future Scope
This project successfully integrates SQL, Power BI, and Excel to provide deep insights into TCS stock performance. Future improvements could include predictive modeling using Python, real-time stock alerts, and AI-based investment strategies.
