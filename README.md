# TimeSeries Prediction

### Task: Load Prediction

### Models implemented: Gradient Boosting Regressor, LSTM, and ARIMA
### Evaluation metrics: Mean Squared Error (MSE), Root Mean Squared Error (RMSE), Mean Absolute Percentage Error (MAPE)

## Data
### Preparing the Data 
- The timestamp is set as the index of the dataframe in datetime format.
- A correlation heatmap is used to discard variables not correlated to the target.
- Observed values are discarded as they are highly correlated to the forecasted values and cannot be used to make predictions.
- An 80:20 split is performed for training and testing.

## ARIMA
It was observed that using an ARIMA model was not feasible as the optimal parameters could not be determined automatically efficiently; upon using the automatically calculated parameters, the model failed to converge. Furthermore, in an attempt to manually select the parameters using the Autocorrelation Function plot, Partial Autocorrelation Function plot, and the Augmented Dickey-Fuller Test, the resultant model seemed very similar to the previously tested ARIMA model. Also, using an ARIMA model for long sequences may not be ideal. Additionally, any other data (e.g. weather data) which may be relevant cannot be used.

## LSTM (Predicting Future Values)
An LSTM was trained to predict the next 160 time steps based on the previous 160 time steps. The value of 160 steps is chosen to allow the model to make predictions at 8:00AM for the same day (64 remaining steps) as well as the next day (96 steps).  
An additional feature was added to the dataset for the LSTM, which is, the load value for the same day of the week in the previous week. The data was scaled using a Min-Max scaler before inputting it to the neural network. Overall, the performance of the LSTM was mediocre even after several attempts at hyperparameter tuning.  

#### Results
- MSE: ≈4270
- RMSE: ≈65
- MAPE: 11.5%

## Gradient Boosting Regressor
This model seemed to performed the best out of all while being simple to implement. The RMSE of the regression model is lower than the LSTM model's error as well as lower than the natural standard deviation of the load values. The forecasted values were used to make predictions for the current event timestamp along with a few other added features. For the regression model, the dataset contained a few features that seemed relevant to the current load based on the trends in the data. The features added were:
- Load from 2 days ago for the current timestamp - a lag of 2 days was chosen as the predictions for tomorrow would be made at 8:00AM today, hence, all the values for 24H ago would not be available.
- Load from the same day of the previous week - a pattern was observed in the load values depending on the day of the week.
- Hour of the day as the load was affected by the time.
- Day of the week.

A feature importance analysis after fitting the model shows that the features engineered and added to the dataset were highly important and relevant to making the predictions.

### Results
- MSE: ≈1022
- RMSE: ≈32
- MAPE: 6.2%

## LSTM (Same data as the Regressor)

Training an LSTM with using the forecasted values along with historic load values to predict the load for the current timestamp delivers better results than using an LSTM to predict future 160 values based on previous 160 inputs. The LSTM is used to predict sequences of 96 timestamps (for one whole day) based on the forecasted weather values and historic load values (48H ago and 7 days ago). However, the Gradient Boosting Regressor appears to have performed better again based on the evaluation metrics. 

### Results
- MSE: 1310 
- RMSE: 36.2
- MAPE: 6.8%

## Conclusion
Three types of models were implemented for the task of load prediction: statistical time series model (ARIMA), Regression model (GBR), and neural network (LSTM). The results suggest that using a regression approach was not only the most efficient but also the most accurate.