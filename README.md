# kaggle-c-revenue
Submission for Google Analytics Customer Revenue Prediction on Kaggle


The data set contains some columns with JSON fields. The fields in JSON format must be converted into columns and added back into the dataframe before proceeding.
The flatten_json funtion handles this issue. This function is applied to both train and test sets.
The modified train and test sets are saved to disk as trainf.csv and testf.csv

There are some columns for which the value for each row is "not available in demo dataset". These columns are irrelevant and are dropped.

Next, a check is done for columns with only 1 unique value. These columns are also irrelevant and can be dropped. 'socialEngagementType','totals.visits' are such columns having only 1 unique value.
A correlation check is performed on the train set to check for highly correlated values. Columns 'totals.pageviews' and 'totals.hits' seem to be highly correlated and one of them can be dropped.

Next, I dropped some columns that seemed to have a lot of missing values to check if the accuracy improved. Dropping these columns did not improve the accuracy.

All unique identifiers and columns such as date and timestamp are dropped.

Some of the values in the columns are categorical i.e they are strings and not numbers. The LabelEncoder function, from the sklearn library is used to encode these categorical values into numerical values.

I tried out a different way to encode these categorical values using the factorize funtion from the pandas library but the accuracy( rmse score) produced by these encoded values were worse than that produced by the LabelEncoder mentioned above. 

All missing values ( represented as NaN) are replaced with 0.0 

Next, training and test sets are made from the train data set.

I tried two different approaches to predict the target variable( totals.transactionRevenue)

The first approach is to use the LinearRegression model from the sklearn library. This yielded an rmse score of 3.32 which is quite high.

The second approach was to use the LightGBM model. I considered xgboost and catboost also for this approach but on further research, found out that lightgbm model would work well for this case.
This model yielded an rmse score of 1.64 on it's best iteration.

Finally, the learned model is used to predict the revenue from each customer in the test set. A dataframe called sub ( for submission) is created with fullvisitorID and corresponding natural log of revenue as the two columns.

This dataframe is saved as sub.csv and submitted to Kaggle.
