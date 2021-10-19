# Crypto-Trend-Prediction

In this repository we'll study the crypto market trying to predict it.

We developed two strategy: the first based on AI models and the second based on Rsi and Macd indicators.

Special thanks to [Carsten](https://www.kaggle.com/tencars/392-crypto-currency-pairs-at-minute-resolution/version/948?select=etheur.csv) for the data collection.

**Disclaimer:**

This is a study made by a student in free time, nothing in this study should be considered as financial advice. 
Invest at your own risk!

## Trend Inversion Point Study

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

Then we calculated some Indicators and add them to our dataframe.

In the end we removed close, high, low and trend columns.


After all our dataframe looks like this:

| time          | open          | macd           | macds         | rsi_12        | max_high     | min_low       | label  |
| ------------- |:-------------:| :-------------:|:-------------:|:-------------:|:------------:|:-------------:|-------:| 
| 149518190000  | 2304.600000   | -0.006397      | -0.009366     |  0.229582     | 1828.183508  | 1793.527485   | 0.0.   |




### Training and Validation

First we removed 3 30 days random samples from our entire dataframe, we will use them for profit validation.

After that we splitted the data in X_test, y_test, X_training, y_training .

Looking at our y_training we noticed that we got almost zeros in it: Counter({0.0: 734424, 1.0: 1044, -1.0: 1092}).
Training the models with that labels will end in a 99% accuracy just because the prediction are ALL zeros.

We used under sampling to avoid that phenomenon.

Now we can finally train our models, and evaluate them:

* Random Forest Balanced Accuracy: 0.7294651491325616
* Decision Tree Balanced Accuracy: 0.680913137946446
* Neural Network balanced accuracy 0.4071340560574852


In the end we evaluated the profit of this prediction in the 3 random samples and compared it with a Baseline Algorithm that buy totaly random and sell if the profit is greater then 3%.

None of the models performed better then the baseline, nor for the profit nor for the average holding time.

The average profit is high, but that's just because of the Crypto Market.

If nothing is wrong then this means none of our model are currenty work.

### Conclusion

Don't use this models in real life investments, because they won't work.
If you want to help with this project feel free to contact me.



## Macd Rsi Indicators

In this section we'll try to study a trading strategy based on indicators and how much we can earn using it.
We'll compare it with an holding strategy and a random strategy, the code is well commented, then just look at it for more info.
