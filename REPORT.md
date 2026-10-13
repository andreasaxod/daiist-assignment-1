# Assignment 1 Report

- **Name**: Andrea Saxod
- **Student ID**: 18996
- **Email**: asaxod.ieu2022@student.ie.edu
- **Group**: BBADBA 5A

## Dataset
The dataset is based on daily OHLCV (Open, High, Low, Close, Volume) data for the S&P 500. The data was retrieved from Yahoo Finance using the yfinance library and covers January 2014 to December 2024 (about 11 years). In terms of raw trading days, we have around 2767, of which 2516 were usable after feature engineering because some features, like 52 Week High, require 252 days of past data before they can be calculated. The raw columns in this dataset are Date, Open, High, Low, Close, and Volume; each row represents one trading day for the S&P 500 index. I chose this dataset because it's challenging to build useful features from raw price data. I started with only 5 numbers per day, and I had to turn them into something the model could learn from. Also, the data split had to be done carefully, as we cannot mix past and future data. 

## Business / real-life framing
Regarding this scenario, we are looking at a trading desk in an investment firm that runs a simple strategy. Every evening, the model observes today's market data and predicts whether the S&P 500 will close higher tomorrow. If our model predicts "Up," the desk buys overnight; if it predicts "Down," the desk does nothing. 

This business framing is what guided the next 3 important decisions of the pipeline:

1) The target was defined as binary: 1 if tomorrow's close is higher than today's close, or 0 if it is lower. This was built by shifting the Close column forward by one day for comparison. However, the very last row of the dataset has no tomorrow, so it gets dropped.

2) To split the data, a time-based split was used. This was the most critical decision in the timeline. If I had randomly shuffled the data, a data point from 2023 could have ended up in the training set while a point from 2020 ended up in the test set. The model would then have learned from the future to predict the past, which is look-ahead bias. In that case, we would probably get very good test results, but the model would be useless in real-world scenarios where we never have tomorrow's data. I split the data into 70% training (2015 to 2021), 15% validation (2022 to mid-2023), and 15% testing (mid-2023 to 2024).

3) Finally, the way in which I think of errors is important, as not all mistakes are equal. A false positive in this case meant the desk bought the index and the market dropped; a real financial loss. A false negative meant the desk stayed out and the market went up; a missed opportunity but with no loss. Because real losses hurt more than a missed gain, I set the cost of false positives at $ 10,000 and false negatives at $5,000. These are purely illustrative numbers that reflect a real trading concept. A conservative desk should raise the decision threshold above 0.5 so the model signals buy only when it is very confident. 

## Data preparation & feature engineering

*What you engineered and why, and any data-quality decisions you made along
the way — e.g. "segment X had defective data, so I excluded it and used a
population-average default for scope Y at inference time; the impact of
that choice is Z."*

## Modeling: three implementations, one model

*Which model (linear or logistic regression) and why. A results table
comparing scikit-learn, the manual PyTorch loop, and the standard
torch.nn.Module/torch.optim workflow, on the same test set, against the
naive baseline. Do the three agree? If not, why not?*

## Limitations & next steps

*Real limitations you found, and concretely how you'd address each one with
more time or data — not generic hedging.*

## Generative AI use disclosure

*Per the syllabus AI Policy: what you used and how, or "no AI content used."*
