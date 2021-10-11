# Crypto-Trend-Prediction

In this repository we'll study the crypto market trying to predict the Trend Inversion Point.

Special thanks to [Carten](https://www.kaggle.com/tencars/392-crypto-currency-pairs-at-minute-resolution/version/948?select=etheur.csv) for the data collection.

### Disclaimer

This is a study made by a student in free time, nothing in this study should be considered as financial advice.


## The study

The purpose of this study is to predict the Cryptocurrency market Trend, we took as example ETH and BTC but it can be used with all coins.

First of all we define the Trend Inversion Point as follow:

The i_th Trend Inversion Point is the local minimum or maximum between the timeframe i and i+1, the TIP can be interpreted as a buy signal if the i_th trend is decrescent and the i+1_th trend is crescent, or as a sell signal if the i_th trend is crescent and the i+1_th trend is decrescent.

### Data Manipulation



Our dataframe for every minutes looks like this:

| time          | open          | close          | high          | low           | volume       |
| ------------- |:-------------:| :-------------:|:-------------:|:-------------:|-------------:| 
| 1495181760000 | 1775.951536   | 1775.951536	   | 1775.951536   | 1775.951536   | 0.010000     |


Some minutes miss in the table, we filled them with the average calculated using the last and next available records.

After that we divided the dataframe into timeframes, we used 180 minutes timeframe but it can be changed.

Then we identified the Trend checking for minimum and maximum in each timeframe, if the minimum appears first then we got a crescent trend, else a decrescent one.

Some timeframe can be full of filled value, in that case we just set the trend to NaN and removed it.

We used the Trend column for identifying the Trend Inversion Point and set the label column, 1 for buy signal, -1 for sell signal, 0 otherwise.

Then we calculated the Exponential Moving Average of open column and volume column and add them to our dataframe.

In the end we removed close, high, low and trend columns.


After all our dataframe looks like this:

| time          | open          | ema            | ema_volume    | volume        | label        |
| ------------- |:-------------:| :-------------:|:-------------:|:-------------:|-------------:| 
| 149518190000  | 2304.600000   | 2301.977373    | 4.259261      |  0.229582     | 0.0          |


## Training and Validation

First we removed 3 random samples from our entire dataframe, we will use them for profit validation.

After that we splitted the data in X_test, y_test, X_training, y_training .

Looking at our y_training we noticed that we got almost zeros in it: Counter({0.0: 734424, 1.0: 1044, -1.0: 1092}).
Training the models with that labels will end in a 99% accuracy just because the prediction are ALL zeros.

We used under sampling to avoid that phenomenon.

Now we can finally train our models, and evaluate them:

* Neural Network, Balanced Accuracy score: 0.43516934942417995
* Decision Tree, Balanced Accuracy score: 0.42751684593313616
* Random Forest, Balanced Accuracy score: 0.4860863078404989


In the end we evaluated the profit of this prediction in the 3 random samples that we removed first:


* Neural Network: TOTAL PROFIT: 0.0
* Decision Tree: TOTAL PROFIT: 0.0
* Random forest: TOTAL PROFIT: 1.066484582450327

As you can see the prediction and profit aren't great.

## Conclusion

Don't use this model in real life investments, because they won't work.
If you want to help with this project feel free to contact me.
