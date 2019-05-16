Predicting Daily Volatility
================
Hyunpyo Kim, Milo Opdahl, Charles Rudolph
May 16, 2019

Abstract
--------

In this project we ask the following question: What if we could predict daily volatility of a company’s stock price and use that to our advantage? We found significance in this question due to our likeminded interest in stock market swings and trends. We created a model that could measure daily volatility using information from the S&P 500 Index and 50 individual companies (selected by market cap). Using a volatility measure of the difference in the log of closing price from day “t” to day “t-1”, we ran lasso regressions to predict the day-ahead volatilities of the top five companies in the S&P 500 (Apple, Microsoft, Amazon, Facebook, and Google). Using results from lasso, we were able to comment on relationships between the top 5 companies’ period t volatilities and the other companies’ t-1 volatilities, as well as form predictive models for the top 5. We followed up by running a train-test split on our models, which showed that in reality our models had minimal support. Although we did find a model that does a better job in predicting volatility, we did not have time to fully incorporate it into our project; however, there is some discussion of this new model in the appendix. Ultimately, this project was an informative exercise into better understanding how difficult it is to predict volatility.

Introduction
------------

Many people get caught up in the impossible idea of conquering the stock market. From the outside looking in, it appears to be a sure method of gaining a lot of money in a short amount of time. In reality, the stock market is quite unpredictable in its movements. A company’s stock may swing up or down in the blink of an eye, making stock direction and price seem impossible to peg for the future. However, what if we could predict the daily volatility of a company’s stock price and use that to our advantage?

On a surface level, volatility may seem worthless: it only measures how much a stock could move in the future, without pinpointing the direction of the movement. However, it is an important measurement in the option’s market, as traders analyze it in order to determine risk level in a particular transaction. For example, high volatility allows for riskier trading opportunities, while low volatility leads to safer but less profitable trades.

Given this, we deem it necessary to be able to create a model that effectively measures volatility. In particular, we will predict day-ahead volatility for individual stocks utilizing yesterday’s volatility for a variety of stocks, with stocks being drawn from the S&P 500. Companies will be ranked by market cap. From there, we will predict day-ahead volatility for the top five companies in this category (Apple, Microsoft, Amazon, Facebook, Google). The remaining companies in the dataset will comprise of the rest of the top 50 companies by market cap. Additionally, we will analyze which individual companies daily volatilities from period t-1 have the strongest effects on each of the top five companies’ daily volatility from period t. Daily volatility will be defined in a simplistic way: the difference in the log of closing price from day “t” to day “t-1”. This is a well-established measurement often utilized by stock market experts. Lastly, we will attempt to observe the viability of our daily volatility models.

Methods
-------

Naturally, an immense amount of financial data was required to create a suitable model. Using the library “Quantmod” in R, we were able to draw data from December 31, 2015 to May 30, 2019 for the 50 companies’ stocks we desired. Within the datasets, numerous daily variables were included, such as opening price, closing price, volume of stock traded, high price, and low price. A “daily volatility” variable was defined using the definition from the introduction. Additionally, we constructed a dataset for Apple, Microsoft, Amazon, Facebook, and Google that compared daily volatility for period “t” to daily volatility for period “t-1” for the rest of the companies.

Once this was organized, we opted to utilize a lasso regression between each of our five companies and the top 50 companies in the S&P 500. We believe that using a regularization technique such as lasso regression was the best option for three reasons. First, using regularization will reduce any overfitting in our stock market data by adding a penalty weight to the fitted betas we wish to find. This will allow for more realism regarding the movements of stock prices for our data. Second, the lasso regression gives sparse solutions in its output. This means that only the most important fitted betas that are not identically zero will be considered through an automatic variable selection. Third, lasso regression performs well when the number of features are small compared to the number of observations. For our stock market dataset, there are certainly many more observations than there are features due to the time-period and the number of companies being observed. To use lasso regression, we utilized the library “gamlr” in R. This library was the best option for lasso regression analysis since it allowed some flexibility in applying model selection rules to our code. After successfully running lasso on the five key companies, we used the beta values to fit a line to the daily volatility plots for the five key companies, which served as our predictive model. (Note: lasso regression plots can be observed in the appendix, figure 5.)

To best understand whether or not our predictive model was viable, we needed to test it under a controlled environment. This was done by doing a train-test split on the predicted models of the top five companies created with the lasso regressions. The train-test split method allows us to see how well out model would perform in an out-of-sample prediction. It is our hope that we are able to find a root means-squared error (RMSE) in our prediction that is lower than the control RMSE.

Results
-------

#### Figure 1: Daily Volatility

![](markdowntest_files/figure-markdown_github/unnamed-chunk-2-1.png)

Figure 1 displays the daily volatility for the previously established “top 5 companies” that we are tracking. For any given company during the chosen period, volatility rarely exceeds 10%. Apple appears to be the most volatile stock on a day-to-day basis.

#### Table 1: Top 5 Most Influential "t-1 Volatilities" on top 5 companies' "t Volatilities"

    ## Loading required package: knitr

|  Company  |          Rank1          |      Rank2      |     Rank3    |      Rank4     |          Rank5          |
|:---------:|:-----------------------:|:---------------:|:------------:|:--------------:|:-----------------------:|
|   Apple   |      Philip\_Morris     | Exxon\_Mobil(-) |    Google    |   Verizon(-)   |     Wells\_Fargo(-)     |
| Microsoft |       Microsoft(-)      |    Pfizer(-)    |    Google    |     PepsiCo    | United\_Technologies(-) |
|   Amazon  |       Home\_Depot       |    Chevron(-)   |    PepsiCo   | Thermo\_Fisher | United\_Technologies(-) |
|  Facebook |          Google         |     Apple(-)    |   Cisco(-)   | Philip\_Morris |       Home\_Depot       |
|   Google  | United\_Technologies(-) |      Google     | Microsoft(-) | Philip\_Morris |        Coca\_Cola       |

The table depicts which companies’ t-1 volatilities best predict the top 5 companies’ volatilities in period t. Companies are listed in order of most predictive to least. Predictive power was determined via analyzing the beta values from lasso, and picking the biggest 5 in terms of absolute value. Company names with a negative sign next to them signal an inverse relationship between t-1 and t volatilities.

#### Figure 2: Predicted Daily Volatility

![](https://raw.githubusercontent.com/crudolph12/hw3/master/doit.png)

Figure 2 displays a predictive line running through the daily volatility. This line was formed by multiplying the beta values obtained from lasso by the t-1 volatilities pertaining to each company with a significant beta.

Conclusion
----------

The table and figures above paint an interesting picture when analyzing daily volatility for the top 5 companies. Figure 1 did a good job of allowing us to visualize just how volatile a stock could be from day to day. It seemed that on an average day volatility could vary by as much as 1 percent, while on a noteworthy day in the company’s history volatility could swing by 10 to 20 percent. As a result of the average days severely outnumbering the shocking days, t-1 volatility from the top 50 companies seems to always predict low volatility days for period t. As can be noted from figure 2, the predicted daily volatility seems to hover around a 1% change, rarely predicting any significant movement. This shortcoming of our model can be explained by the variables we chose to predict with: t-1 volatilities from the 50 companies has no way to account for surprise events that will happen tomorrow. However, predicted volatility did fit daily volatility for Facebook rather well , especially from late 2017 to early 2018. During this period, Facebook’s daily volatility was very low. So, it appears our model can do a great job of predicting stocks with generally low volatilities.

The table provides insight into which companies’ t-1 volatilities the lasso determined were most predictive of the top 5 companies’ t volatilities. In general, it seemed companies with established relationships or that resided in similar industries predicted each other well. For example, t-1 daily volatility for Google was the best predictor for t daily volatility for Facebook. Given that they are two of the most popular websites in the world, it is only natural that their two’s volatilities are intertwined. Another interesting point was the common occurrence of inverse relationships between the top 5 and top 50 companies’ volatilities. An inverse relationship meant that as the t-1 volatility for one of the 50 companies increased/decreased, the predicted t volatility of the top 5 company in question moved in the opposite direction. An example of this occurred when discussing the relationship between Apple and Exxon Mobil. One potential explanation for this may be that extreme volatility in an energy market such as Exxon Mobil could leave the public in a financially unstable place, resulting in less purchases of the often expensive technology that Apple provides. In turn, fewer purchases could feasibly lower daily volatility. Additionally, further consideration of the observed inverse volatility relationships should remind us of some aspects of correlation analysis. One thought is that correlation does not necessarily imply causation. Indeed, the previously mentioned volatility relationship between Apple and Exxon Mobil may not necessarily have a direct link to one another. Perhaps there are other factors that we have not considered in our model that explain why the volatility of these two companies happen to be inverse.

Two strange results stood out to us: Microsoft’s inverse relationship with itself, and the prevalence of Philip Morris. Both are difficult to interpret and fuel suspicion towards our model’s legitimacy. Perhaps the inverse relationship could be explained by Microsoft stock trying to correct towards a certain benchmark, such as 0% volatility. Philip Morris’ constant presence in the rankings is easier to explain. By observing the daily volatility of the company (seen in the appendix, figure 3), it is clear that Philip Morris is a company with minimal daily volatility. This indicates the model may be tending towards keeping daily volatility predictions low, giving credence to discussions of the same topic earlier in the conclusion.

Given the often strange results of our table, we found it imperative to run train test splits on our data. We created a test split that featured 2019 data, and proceeded to run k-fold lasso regression on the splits on each of the top 5 companies. Unsurprisingly, we found very few meaningful results. It appeared that the usage of t-1 company volatility was a poor predictor of t volatility. In the big picture, this certainly makes sense. As stated earlier in the report, anything involving stock would be near impossible to predict. Such a simplistic model was bound to run into its fair share of issues. Certainly, there are a multitude of other factors that could possibly explain the daily volatility of companies in the S&P 500. However, we do not feel the project was a failure. Our measure of daily volatility from figure 1 are useful in their own right, and a few of the relationships featured in the table seemed legitimate.

Feeling inspired to create at least one suitable model, we were able to form a predictive model involving daily volatility drawing from more conventional methods that exceeded expectations for train test splits and k-fold lasso regression. Sadly, we did not have time to conduct a proper analysis and interpretation of the new model; however, perhaps a future in-depth inquiry to this model would be of use for us when considering daily volatility. This model can be viewed in the appendix, Figure 4.

Although we were unsuccessful in creating a perfect model, this project was useful in showing the difficulties faced when trying to create a model that would predict daily volatility. We hope that in future considerations, we may be able to use our new model to better interpret how volatility takes place in the stock market.

Appendix
--------

#### Figure 3:Philip Morris Daily Volatility

![](markdowntest_files/figure-markdown_github/unnamed-chunk-9-1.png)

This figure confirms the hypothesis that Philip Morris has minimal daily volatility throughout the time period chosen for the project, lending credence to the idea that our predictive model prefered a low, constant volatility.

#### Figure 4: Conventianal Model for predicting Daily Volatility

Our new model focuses on using conventional predictive variables to estimate daily volatility. In order to form an effective model, we opted to utilize a train test split, with the train drawing from 2016-2018 data and the test attempting to predict 2019 data. Our measurement of daily volatility will stay the same as our initial experiment, (log(close(t))/log(close(t-1))).

The variables we decided to use are as follows:

1.  stock’s price and volume data : open, high, low. close, volume and adjusted price
2.  Moving average of closing price(5 days, 50 days, 200 days) :
    -   difference between MA line and closing price : log(closing price(t)/MA50(t))
    -   daily moving trends of MA line : log(MA50(t)/(MA50(t-1))
3.  Golden Cross/Death Cross signal (the point where 50 days MA line and 200 days MA line crosses)
4.  Historical high and low : the highest and lowest closing price within the past 52 weeks (1 year), the proportional closing price within the highest and lowest price
5.  Candlestick : the gap between high and low price, and the gap between open and close price
6.  Historical volatility : 21 days and 5 days historical volatility ( close to close method)
7.  VIX : high and close, drawn from the VIX
8.  Industry : The daily volatilities of stocks in the same industry
9.  Major indices : S&P500, Dow30, NASDAQ, Russell 2000
10. ETF for world market : world(without US), South America, China, Japan, Hong Kong, India, Germany, UK, France
11. Meaningful days of week : Monday and Friday, which experience different trends than other days.

After establishing this list, we ran a lasso regression on the train test split for many companies. As expected, our model can’t always explain the stock’s volatility. Often, lasso models selected by cross validation contained only the intercept, meaning the average would be better than the model we formed. However, some stocks were able to be predicted effectively. A specific example of this was the predictive model for Microsoft.

![](markdowntest_files/figure-markdown_github/unnamed-chunk-11-1.png)

The plot above displays daily volatility in black, predicted daily volatility via lasso in red(y\_hat1) and predicted daily volatility via cross validated lasso in blue (y\_hat2). It seems that the predicted lines do a respectable job of predicting volatility. The RMSE values reveal that the lasso model selected by cross validation is better than that selected by AIC and the model for 0.

``` r
c(RMSE(Y_test,Yhat), RMSE(Y_test,YCVhat), RMSE(Y_test,0))
```

    ## [1] 0.01299 0.01288 0.01352

#### Figure 5: Lasso Regression Plots for the top 5 companies

![](https://raw.githubusercontent.com/crudolph12/hw3/master/lasso.png)

The dotted vertical line represents the optimal level for log lambda, which minimizes AICc. The number of colored lines intersecting this represent the number of betas significant at the given lambda value. This method was how we determined which betas would be utilized in our predictive model for daily volatility, as seen in figure 2.
