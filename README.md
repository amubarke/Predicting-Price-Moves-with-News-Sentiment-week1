Week-1 Stock Movement & News Sentiment Analysis

A data-driven exploration of how daily news sentiment correlates with AAPL stock movements.
This project analyzes financial news headlines, performs sentiment scoring, computes stock returns, and investigates correlations between market behavior and public sentiment.


Project Overview

This Week-1 deliverable covers:

✔️ Sentiment Analysis

Text cleaning

Polarity scoring using TextBlob

Categorizing sentiment into Positive, Negative, and Neutral

Aggregating average daily sentiment

✔️ Stock Price Analysis

Downloading historical stock data with yfinance

Cleaning and normalizing date formats

Calculating Daily Returns (% change)

Aligning stock market dates with available news dates

✔️ Correlation Analysis

Correlation between:

Same-day sentiment vs closing price change

Next-day sentiment vs stock returns

Handling mismatched dates (market closed vs general news cycle)

✔️ Visualizations

Sentiment distribution bar charts

Time-series sentiment trends

Stock closing price trends

Daily return plots

Sentiment × Price correlation charts


Project Structure
📦 week-1-news-stock-analysis
┣ 📂 data
┃ ┣ headlines.csv
┃ ┗ processed_sentiment.csv
┣ 📂 notebooks
┃ ┗ analysis.ipynb
┣ 📂 plots
┃ ┣ sentiment_distribution.png
┃ ┣ stock_trend.png
┃ ┗ correlation_chart.png
┣ README.md
┗ requirements.txt


Methods Used
1. Sentiment Scoring
from textblob import TextBlob

headlines["polarity"] = headlines["headline"].apply(lambda x: TextBlob(str(x)).sentiment.polarity)

def categorize_score(x):
    if x > 0.05:
        return "Positive"
    elif x < -0.05:
        return "Negative"
    else:
        return "Neutral"

headlines["sentiment"] = headlines["polarity"].apply(categorize_score)


2. Daily Sentiment Aggregation
daily_sentiment = headlines.groupby("Date")["polarity"].mean().reset_index()
daily_sentiment.rename(columns={"polarity": "avg_daily_sentiment"}, inplace=True)

3. Stock Data Processing
import yfinance as yf
import pandas as pd

start_date = "2019-09-03"
end_date   = "2020-06-05"

stock = yf.download("AAPL", start=start_date, end=end_date)
stock = stock.reset_index()

stock["Date"] = pd.to_datetime(stock["Date"]).dt.tz_localize(None)
stock["Daily_Return"] = stock["Close"].pct_change() * 100


4. Merge & Correlation
merged = pd.merge(stock, daily_sentiment, on="Date", how="inner")

merged[["Daily_Return", "avg_daily_sentiment"]].corr()

Key Findings

🔹 Sentiment tends to move ahead of stock price movements.
🔹 Same-day correlation is typically weak because the market reflects news with slight delay.
🔹 Next-day sentiment correlation is stronger (varies by dataset).
🔹 News dataset often contains weekends, while markets do not → requires normalization.
