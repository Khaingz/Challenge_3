# Challenge_3

Crypto Arbitrage

In this Challenge, you'll take on the role of an analyst at a high-tech investment firm. The vice president (VP) of your department is considering arbitrage opportunities in Bitcoin and other cryptocurrencies. As Bitcoin trades on markets across the globe, can you capitalize on simultaneous price dislocations in those markets by using the powers of Pandas?

For this assignment, you’ll sort through historical trade data for Bitcoin on two exchanges: Bitstamp and Coinbase. Your task is to apply the three phases of financial analysis to determine if any arbitrage opportunities exist for Bitcoin.

This aspect of the Challenge will consist of 3 phases.

Collect the data.

Prepare the data.

Analyze the data.


Import the required libraries and dependencies.

import pandas as pd
import numpy as np
from pathlib import Path
%matplotlib inline

Collect the Data
To collect the data that you’ll need, complete the following steps:

Instructions.

Using the Pandas read_csv function and the Path module, import the data from bitstamp.csv file, and create a DataFrame called bitstamp. Set the DatetimeIndex as the Timestamp column, and be sure to parse and format the dates.

Use the head (and/or the tail) function to confirm that Pandas properly imported the data.

Repeat Steps 1 and 2 for coinbase.csv file.


Step 1: Using the Pandas read_csv function and the Path module, import the data from bitstamp.csv file, and create a DataFrame called bitstamp. Set the DatetimeIndex as the Timestamp column, and be sure to parse and format the dates.
# Read in the CSV file called "bitstamp.csv" using the Path module. 
# The CSV file is located in the Resources folder.
# Set the index to the column "Date"
# Set the parse_dates and infer_datetime_format parameters

bitstamp_df = pd.read_csv(
    Path("./Resources/bitstamp.csv"),
        index_col="Timestamp",
        parse_dates=True,
        infer_datetime_format=True)
        
        
Step 2: Use the head (and/or the tail) function to confirm that Pandas properly imported the data.
# Use the head (and/or tail) function to confirm that the data was imported properly.
bitstamp_df.tail()


Step 3: Repeat Steps 1 and 2 for coinbase.csv file.
# Read in the CSV file called "coinbase.csv" using the Path module. 
# The CSV file is located in the Resources folder.
# Set the index to the column "Timestamp"
# Set the parse_dates and infer_datetime_format parameters
coinbase_df = pd.read_csv(
    Path("./Resources/coinbase.csv"),
    index_col="Timestamp",
    parse_dates=True,
    infer_datetime_format=True)
    
# Use the head (and/or tail) function to confirm that the data was imported properly.
coinbase_df.tail()


Prepare the Data
To prepare and clean your data for analysis, complete the following steps:

For the bitstamp DataFrame, replace or drop all NaN, or missing, values in the DataFrame.

Use the str.replace function to remove the dollar signs ($) from the values in the Close column.

Convert the data type of the Close column to a float.

Review the data for duplicated values, and drop them if necessary.

Repeat Steps 1–4 for the coinbase DataFrame.

Step 1: For the bitstamp DataFrame, replace or drop all NaN, or missing, values in the DataFrame.
# For the bitstamp DataFrame, replace or drop all NaNs or missing values in the DataFrame
bitstamp_df.isnull().sum()
bitstamp_df = bitstamp_df.dropna()
bitstamp_df.isnull().sum()

Step 2: Use the str.replace function to remove the dollar signs ($) from the values in the Close column.
# Use the str.replace function to remove the dollar sign, $
bitstamp_df.loc[:, "Close"]=bitstamp_df.loc[:, "Close"].str.replace("$","",regex=True)

Step 3: Convert the data type of the Close column to a float.
# Convert the Close data type to a float
bitstamp_df.loc[:,"Close"] = bitstamp_df.loc[:, "Close"].astype("float")
# Check data type
bitstamp_df["Close"].dtypes
# check the data became float
bitstamp_df.tail()

Step 4: Review the data for duplicated values, and drop them if necessary.
# Review the data for duplicate values, and drop them if necessary
bitstamp_df = bitstamp_df.drop_duplicates()
bitstamp_df.duplicated().sum()
bitstamp_df

Step 5: Repeat Steps 1–4 for the coinbase DataFrame.
# Repeat Steps 1–4 for the coinbase DataFrame
coinbase_df.isnull().sum()
coinbase_df = coinbase_df.dropna()
coinbase_df.isnull().sum()
coinbase_df.loc[:, "Close"]=coinbase_df.loc[:, "Close"].str.replace("$","",regex=True)
coinbase_df.loc[:,"Close"] = coinbase_df.loc[:, "Close"].astype("float")
coinbase_df.tail()
coinbase_df = coinbase_df.drop_duplicates()
coinbase_df.duplicated().sum()

Analyze the Data
Your analysis consists of the following tasks:

Choose the columns of data on which to focus your analysis.

Get the summary statistics and plot the data.

Focus your analysis on specific dates.

Calculate the arbitrage profits.

Step 1: Choose columns of data on which to focus your analysis.
Select the data you want to analyze. Use loc or iloc to select the following columns of data for both the bitstamp and coinbase DataFrames:

Timestamp (index)
Close

# Use loc or iloc to select `Timestamp (the index)` and `Close` from bitstamp DataFrame
bitstamp_sliced = bitstamp_df.loc[:,"Close"]
# Review the first five rows of the DataFrame
bitstamp_sliced.head()

# Use loc or iloc to select `Timestamp (the index)` and `Close` from coinbase DataFrame
coinbase_sliced = coinbase_df.loc[:, "Close"]

# Review the first five rows of the DataFrame
coinbase_sliced.head()

Step 2: Get summary statistics and plot the data.
Sort through the time series data associated with the bitstamp and coinbase DataFrames to identify potential arbitrage opportunities. To do so, complete the following steps:

Generate the summary statistics for each DataFrame by using the describe function.

For each DataFrame, create a line plot for the full period of time in the dataset. Be sure to tailor the figure size, title, and color to each visualization.

In one plot, overlay the visualizations that you created in Step 2 for bitstamp and coinbase. Be sure to adjust the legend and title for this new visualization.

Using the loc and plot functions, plot the price action of the assets on each exchange for different dates and times. Your goal is to evaluate how the spread between the two exchanges changed across the time period that the datasets define. Did the degree of spread change as time progressed?

bitstamp_sliced.describe()
# Generate the summary statistics for the bitstamp DataFrame
bitstamp_sliced.describe()

# Generate the summary statistics for the coinbase DataFrame
coinbase_sliced.describe()

# Create a line plot for the bitstamp DataFrame for the full length of time in the dataset 
# Be sure that the figure size, title, and color are tailored to each visualization
bitstamp_sliced.plot(figsize=(6, 5), title="bitstamp", color="blue")

# Create a line plot for the coinbase DataFrame for the full length of time in the dataset 
# Be sure that the figure size, title, and color are tailored to each visualization
coinbase_sliced.plot(figsize=(6, 5), title="coinbase", color="Orange")

# Overlay the visualizations for the bitstamp and coinbase DataFrames in one plot
# The plot should visualize the prices over the full lenth of the dataset
# Be sure to include the parameters: legend, figure size, title, and color and label
bitstamp_sliced.plot(legend=True, figsize=(15, 7), title="bitstamp and coinbase Overlay Plot", color="blue", label="bitstamp")
coinbase_sliced.plot(legend=True, figsize=(15, 7), color="orange", label="coinbase")

# Using the loc and plot functions, create an overlay plot that visualizes 
# the price action of both DataFrames for a one month period early in the dataset
# Be sure to include the parameters: legend, figure size, title, and color and label
bitstamp_sliced.loc['2018-01-01'].plot(legend=True, figsize=(15, 7), title="Jan-2018", color="blue", label="bitstamp")
coinbase_sliced.loc['2018-01-01'].plot(legend=True, figsize=(15, 7), color="orange", label="coinbase")

# Using the loc and plot functions, create an overlay plot that visualizes 
# the price action of both DataFrames for a one month period later in the dataset
# Be sure to include the parameters: legend, figure size, title, and color and label 
bitstamp_sliced.loc['2018-03-01'].plot(legend=True, figsize=(15, 7), title="March-2018", color="blue", label="bitstamp")
coinbase_sliced.loc['2018-03-01'].plot(legend=True, figsize=(15, 7), color="orange", label="coinbase")

**Question** Based on the visualizations of the different time periods, has the degree of spread change as time progressed?

**Answer** YOUR ANSWER HERE
NO

Step 3: Focus Your Analysis on Specific Dates
Focus your analysis on specific dates by completing the following steps:

Select three dates to evaluate for arbitrage profitability. Choose one date that’s early in the dataset, one from the middle of the dataset, and one from the later part of the time period.

For each of the three dates, generate the summary statistics and then create a box plot. This big-picture view is meant to help you gain a better understanding of the data before you perform your arbitrage calculations. As you compare the data, what conclusions can you draw?

# Create an overlay plot that visualizes the two dataframes over a period of one day early in the dataset. 
# Be sure that the plots include the parameters `legend`, `figsize`, `title`, `color` and `label` 
bitstamp_sliced.loc['2018-01-5'].plot(legend=True, figsize=(15, 7), title="Jan-5-2018", color="blue", label="bitstamp")
coinbase_sliced.loc['2018-01-5'].plot(legend=True, figsize=(15, 7), color="orange", label="coinbase")

# Using the early date that you have selected, calculate the arbitrage spread 
# by subtracting the bitstamp lower closing prices from the coinbase higher closing prices
arbitrage_spread_early = bitstamp_df['Close'].loc['2018-01-5']-coinbase_df['Close'].loc['2018-01-5']
# Generate summary statistics for the early DataFrame
arbitrage_spread_early.describe()

# Visualize the arbitrage spread from early in the dataset in a box plot
arbitrage_spread_early.plot(kind="box")

# Create an overlay plot that visualizes the two dataframes over a period of one day from the middle of the dataset. 
# Be sure that the plots include the parameters `legend`, `figsize`, `title`, `color` and `label` 
bitstamp_sliced.loc['2018-02-18'].plot(legend=True, figsize=(15, 7), title="Feb-18-2018", color="blue", label="bitstamp")
coinbase_sliced.loc['2018-02-18'].plot(legend=True, figsize=(15, 7), color="orange", label="coinbase")

# Using the date in the middle that you have selected, calculate the arbitrage spread 
# by subtracting the bitstamp lower closing prices from the coinbase higher closing prices
arbitrage_spread_middle = bitstamp_df['Close'].loc['2018-02-18']-coinbase_df['Close'].loc['2018-02-18']

# Generate summary statistics 
arbitrage_spread_middle.describe()

# Visualize the arbitrage spread from the middle of the dataset in a box plot
arbitrage_spread_middle.plot(kind="box")

# Create an overlay plot that visualizes the two dataframes over a period of one day from late in the dataset. 
# Be sure that the plots include the parameters `legend`, `figsize`, `title`, `color` and `label` 
bitstamp_sliced.loc['2018-03-28'].plot(legend=True, figsize=(15, 7), title="March-28-2018", color="blue", label="bitstamp")
coinbase_sliced.loc['2018-03-28'].plot(legend=True, figsize=(15, 7), color="orange", label="coinbase")

# Using the date from the late that you have selected, calculate the arbitrage spread 
# by subtracting the bitstamp lower closing prices from the coinbase higher closing prices
arbitrage_spread_late = bitstamp_df['Close'].loc['2018-03-28']-coinbase_df['Close'].loc['2018-03-28']

# Generate summary statistics for the late DataFrame
arbitrage_spread_late.describe()

# Visualize the arbitrage spread from late in the dataset in a box plot
arbitrage_spread_late.plot(kind="box")

Step 4: Calculate the Arbitrage Profits
Calculate the potential profits for each date that you selected in the previous section. Your goal is to determine whether arbitrage opportunities still exist in the Bitcoin market. Complete the following steps:

For each of the three dates, measure the arbitrage spread between the two exchanges by subtracting the lower-priced exchange from the higher-priced one. Then use a conditional statement to generate the summary statistics for each arbitrage_spread DataFrame, where the spread is greater than zero.

For each of the three dates, calculate the spread returns. To do so, divide the instances that have a positive arbitrage spread (that is, a spread greater than zero) by the price of Bitcoin from the exchange you’re buying on (that is, the lower-priced exchange). Review the resulting DataFrame.

For each of the three dates, narrow down your trading opportunities even further. To do so, determine the number of times your trades with positive returns exceed the 1% minimum threshold that you need to cover your costs.

Generate the summary statistics of your spread returns that are greater than 1%. How do the average returns compare among the three dates?

For each of the three dates, calculate the potential profit, in dollars, per trade. To do so, multiply the spread returns that were greater than 1% by the cost of what was purchased. Make sure to drop any missing values from the resulting DataFrame.

Generate the summary statistics, and plot the results for each of the three DataFrames.

Calculate the potential arbitrage profits that you can make on each day. To do so, sum the elements in the profit_per_trade DataFrame.

Using the cumsum function, plot the cumulative sum of each of the three DataFrames. Can you identify any patterns or trends in the profits across the three time periods?

(NOTE: The starter code displays only one date. You'll want to do this analysis for two additional dates).

1. For each of the three dates, measure the arbitrage spread between the two exchanges by subtracting the lower-priced exchange from the higher-priced one. Then use a conditional statement to generate the summary statistics for each arbitrage_spread DataFrame, where the spread is greater than zero.
NOTE: For illustration, only one of the three dates is shown in the starter code below.

arbitrage_spread_early
# For the date early in the dataset, measure the arbitrage spread between the two exchanges
# by subtracting the lower-priced exchange from the higher-priced one
arbitrage_spread_early = bitstamp_df['Close'].loc['2018-01-5']-coinbase_df['Close'].loc['2018-01-5']
# Use a conditional statement to generate the summary statistics for each arbitrage_spread DataFrame
arbitrage_spread_early = arbitrage_spread_early[arbitrage_spread_early > .01]
arbitrage_spread_early.describe()

# For the date early in the dataset, measure the arbitrage spread between the two exchanges
# by subtracting the lower-priced exchange from the higher-priced one
arbitrage_spread_middle = bitstamp_df['Close'].loc['2018-02-18']-coinbase_df['Close'].loc['2018-02-18']
# Use a conditional statement to generate the summary statistics for each arbitrage_spread DataFrame
arbitrage_spread_middle = arbitrage_spread_middle[arbitrage_spread_middle > .01]
arbitrage_spread_middle.describe()

# For the date early in the dataset, measure the arbitrage spread between the two exchanges
# by subtracting the lower-priced exchange from the higher-priced one
arbitrage_spread_late = bitstamp_df['Close'].loc['2018-03-28']-coinbase_df['Close'].loc['2018-03-28']
# Use a conditional statement to generate the summary statistics for each arbitrage_spread DataFrame
arbitrage_spread_late = arbitrage_spread_late[arbitrage_spread_late > .01]
arbitrage_spread_late.describe()

2. For each of the three dates, calculate the spread returns. To do so, divide the instances that have a positive arbitrage spread (that is, a spread greater than zero) by the price of Bitcoin from the exchange you’re buying on (that is, the lower-priced exchange). Review the resulting DataFrame.
# For the date early in the dataset, calculate the spread returns by dividing the instances when the arbitrage spread is positive (> 0) 
# by the price of Bitcoin from the exchange you are buying on (the lower-priced exchange).
spread_return_early= arbitrage_spread_early[arbitrage_spread_early>0] / bitstamp_df["Close"].loc["2018-01-5"]

# Review the spread return DataFrame
spread_return_early.head()

# For the date middle in the dataset, calculate the spread returns by dividing the instances when the arbitrage spread is positive (> 0) 
# by the price of Bitcoin from the exchange you are buying on (the lower-priced exchange).
spread_return_middle= arbitrage_spread_middle[arbitrage_spread_middle>0] / bitstamp_df["Close"].loc["2018-02-18"]

# Review the spread return DataFrame
spread_return_middle.head()

# For the date late in the dataset, calculate the spread returns by dividing the instances when the arbitrage spread is positive (> 0) 
# by the price of Bitcoin from the exchange you are buying on (the lower-priced exchange).
spread_return_late= arbitrage_spread_late[arbitrage_spread_late>0] / bitstamp_df["Close"].loc["2018-03-28"]

# Review the spread return DataFrame
spread_return_late.head()

3. For each of the three dates, narrow down your trading opportunities even further. To do so, determine the number of times your trades with positive returns exceed the 1% minimum threshold that you need to cover your costs.
# For the date early in the dataset, determine the number of times your trades with positive returns 
# exceed the 1% minimum threshold (.01) that you need to cover your costs
profitable_trades_early = spread_return_early[spread_return_early > .01]

# Review the first five profitable trades
profitable_trades_early.head()

# For the date middle in the dataset, determine the number of times your trades with positive returns 
# exceed the 1% minimum threshold (.01) that you need to cover your costs
profitable_trades_middle= spread_return_middle[spread_return_middle > .01]

# Review the first five profitable trades
profitable_trades_middle.head()

# For the date late in the dataset, determine the number of times your trades with positive returns 
# exceed the 1% minimum threshold (.01) that you need to cover your costs
profitable_trades_late= spread_return_late[spread_return_late > .01]

# Review the first five profitable trades
profitable_trades_late.head()

4. Generate the summary statistics of your spread returns that are greater than 1%. How do the average returns compare among the three dates?
# For the date early in the dataset, generate the summary statistics for the profitable trades
# or you trades where the spread returns are are greater than 1%
profitable_trades_early = spread_return_early[spread_return_early < .01]
profitable_trades_early.describe()

# For the date middle in the dataset, generate the summary statistics for the profitable trades
# or you trades where the spread returns are are greater than 1%
profitable_trades_middle = spread_return_middle[spread_return_middle < .01]
profitable_trades_middle.describe()

# For the date late in the dataset, generate the summary statistics for the profitable trades
# or you trades where the spread returns are are greater than 1%
profitable_trades_late = spread_return_late[spread_return_late < .01]
profitable_trades_late.describe()

5. For each of the three dates, calculate the potential profit, in dollars, per trade. To do so, multiply the spread returns that were greater than 1% by the cost of what was purchased. Make sure to drop any missing values from the resulting DataFrame.
# Define each of the three dates dateframe: 
profit_per_trade_early = profit_early.dropna()
# For the date early in the dataset, calculate the potential profit per trade in dollars 
# Multiply the profitable trades by the cost of the Bitcoin that was purchased
profit_early = profitable_trades_early * bitstamp_df['Close'].loc['2018-01-5']

# Drop any missing values from the profit DataFrame
profit_per_trade_early = profit_early.dropna()

# View the early profit DataFrame
profit_early.head()

6. Generate the summary statistics, and plot the results for each of the three DataFrames.
# Generate the summary statistics for the early profit per trade DataFrame
# Define each of the three dates dateframe:

profit_per_trade_early = profit_early.dropna()
profit_per_trade_early.describe()

7. Calculate the potential arbitrage profits that you can make on each day. To do so, sum the elements in the profit_per_trade DataFrame.
# Define each of the three dates dateframe:
# Calculate the sum of the potential profits for the early profit per trade DataFrame
profit_sum = profit_per_trade_early.sum()
profit_sum

8. Using the cumsum function, plot the cumulative sum of each of the three DataFrames. Can you identify any patterns or trends in the profits across the three time periods?
# Use the cumsum function to calculate the cumulative profits over time for the early profit per trade DataFrame
cumulative_profit_early = profit_per_trade_early.cumsum()
Cumulative Early Profits
# Plot the cumulative sum of profits for the early profit per trade DataFrame
cumulative_profit_early.plot(figsize=(10, 7), title="Cumulative Early Profits")

Question: After reviewing the profit information across each date from the different time periods, can you identify any patterns or trends?

Answer: Yes, after reviewing the profit information across each date from the different time periods, i can clearly identify Cumulative Early Profits data.
From the plot, we can see that profits accumulated at a consistent pace in the early morning, started to level off in the evening, and then topped out once the trades ceased to be profitable.