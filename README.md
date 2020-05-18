### Link to Medium blog: https://medium.com/@chris.s.park/forecasting-influenza-related-pneumonia-mortality-using-sarima-856a05c330cf

I acquired data from the CDC’s National Center for Health Statistics (NCHS) mortality surveillance data. The data contained 341 observations, beginning at 2013–10–07 and ending 2020–04–20. Pneumonia deaths were reported if they had been associated with influenza ICD-10 codes.

After cleaning the data set, I ran a naive plot documenting the trend of pneumonia mortality for the past 6 cycles.

Other than the 2020 cycle, the time series displays a clear seasonal trend of influenza-related pneumonia deaths, which would align with existing knowledge of seasonal influenza trends. And, since there is a seasonal component to this time series, it should be non-stationary, however it should be confirmed through tests and visualization.

Although, visually there is evidence of a seasonal trend and the ACF plot displays a gradual decline commonly seen in non-stationary data. The ADF test (p < 0.001) rejects the null hypothesis (or rejects the presence of Unit Root), thus claiming the time series is stationary, despite accounting for lag.
I hypothesized that these conflicting figures may have been a result of the 2020 cycle acting as an ‘outlier’. So, I decided to ‘split train test’ my time series data (testing data — being most recent 2 cycles; training data —being the first 5 cycles), and perform the same tests on my training data, which would subsequently have the 2020 cycle removed.


### Time series and Auto-Correlated Plots of Single-Differenced Data

Running the ADF test on training data (first 5 cycles) presented statistically insignificant results (p=0.569). Therefore, with the backing of the ACF plots data is non-stationary, I performed a single difference transformation on the time series which was found to be statistically significant (p=0.002). Our time series data is now stationary and ready for modeling.
Now, for model selection, I decided to run SARIMA, using auto ARIMA for parameter selection (optimized for AIC). Auto ARIMA will output parameters that rendered lowest AIC or best model fit through running many different models on combination of parameters.

Other than a few residual errors, the residuals seem to fluctuate around a mean of zero and have an okay uniform variance. The density plot suggest normal distribution with mean zero with slight positive skewness, which was backed by the QQ plot. The ACF plot shows the residual errors are not autocorrelated. Any autocorrelation would imply that there is some pattern in the residual errors which are not explained in the model. Not the greatest, but it seems okay, so I went ahead and produced the SARIMA model using the following parameters (p=3, d=1, q=3) x (P=0, D=1, Q=3, s=5).

SARIMA model MSE:564706.4129499898
'mape': 0.09784759059471684

Around 9.8% MAPE implies the model is about 90.2% accurate in predicting the next 2 cycles. The MSE was 564,706, which is quite large, however, when you take into account of the large spike in the 2020 cycle, a larger MSE should’ve been expected.

### Conclusion

While, the model predictions for 2019 cycle was pretty accurate when compared to actual data, the 2020 cycle that was already known to be a severe flu season had failed to be captured by the model. Overall, the SARIMA model was able to adequately forecast influenza-related pneumonia deaths. While the 2019 cycle recorded one of the lowest deaths in the past 6 years, the 2020 cycle saw an increased surge of influenza-related pneumonia deaths. The timing of both the severe 2020 flu season and covid-19 outbreak is very interesting and even a little suspect (my opinions), perhaps once more covid-19 data becomes available, future studies can investigate whether there were any confounders or biases apparent in this data.
