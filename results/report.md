# Credit_Card_Fraud_Detection-Kaggle

Today more and more people are using credit cards or debit cards for payments. They are really convenient and make our life easier. All we need to do is carry only enough cash for small daily payments and doing unplanned and bigger payments with our cards.
Debit  cards are connected to our bank deposit accounts, that we keep our cash in.Credit cards have their own credit limits that are assigned by banks. When we pay with our credit or debit cards, we spend the money that we earned hardly or the credit limit that we probably payback with small interest.

As always, somebody might be interested in our pocket. However thieves don’t do it obviously like they used to be. They copy our plastic cards while we are using them in an ATM or steal our card numbers during an online purchase. They have their own tricks to rob us. When they have our card information or copy of our cards, they use them as fast as possible until we or our card providers realize the situation and block the transactions.

The unauthorized usage of cards is termed as fraud.

Fraud activities and the loss they caused are critical issues for credit card providers. Companies have their own fraud mechanisms, which scan all card transactions real or nearly real time. The purpose of that fraud mechanism is to detect the fraud activities and to protect their customers, their merchants and also themselves. When a card transaction is suspicious, the fraud mechanism creates an alert. Usually a fraud specialist investigates this suspicious card transaction and makes a final decision whether the transaction is fraud or not. And according to this decision, next actions are taken. 

Those fraud mechanisms and how accurate they create those alerts are critical for the health of the system. If they create too many alerts, that are mostly false, then fraud specialists have to check too many transactions or they are unable to do them all.

Therefore, these fraud mechanisms and how accurate their alerts are seriously critical.

In this study, I worked on a credit card transaction dataset which contains 284k rows records with 31 features:

- 28 anonymous features, aliased as v1-v28
- Time feature represents that number of seconds elapsed between this transaction and the first transaction in the dataset
- Amount feature represents the purchase amount.
- Class feature represents whether the transaction is fraud or not.

The purpose of this study is to create a predictive model to classify the transactions.

At the beginning, I started with exploratory data analysis and tried to understand features and how they differ from each class. Descriptive statistics were calculated and plots were made to understand the shape of the data.

The findings of the exploratory data analysis step:
- Time feature contains 2 following days data. 
  - When I checked the minimum and maximum values of this feature, they were 0 and 172k. And then I divided the max value to 60 and converted it to minutes. Then I divided the result to 60 one more time and converted it to hours and found 48. Time values were not directly useful for this study. I converted seconds data to hours by thinking that a transaction early in the morning or late at night might be a sign of fraud. 
  - The histogram plot of this feature supports this idea.
 ![Histogram](/images/Graph_1_Time_histogram.png)
  - With the thought of transaction hours might be correlated, I converted this time data to hourly data(eg. 16:00,17:00) and added it to the dataset.
- Anonymous features, v1-v28, all have extreme values.
  - The descriptive statistics of those features had too low or too high min and max values. And when I compared min and max values of each feature with the first and last quartile values, there was an obvious spike that stands between.
  - At this point I decided to handle this extreme value issue in different ways and would like to compare the effects of each on the predictive modelling process. The way I modified extreme values and how I created 4 datasets:
    - Dataset-1: No modification of extreme values.
    - Dataset-2: Extreme values were flagged.
    - Dataset-3: Extreme values were removed.
    - Dataset-4: Extreme values were replaced and flagged.

After the explanatory data step and prepared 4 dataset, I started the predictive modelling step. I chose to work with 2 algorithms, logistic regression and decision tree. My plans were to run those algorithms on 4 datasets, finding good classifiers for each dataset and compare their results with each other. But the goal was not only finding a good classifier and also reviewing the results to understand which extreme value modification technique makes difference ? 

I used the grid search cv in the sklearn library and found relatively best parameters for each dataset and algorithm combinations. As I was running the grid search cv, I chose to use F1 score to find a successful predictive mode. F1 Score is used to find a predictor with the best precision and recall score combination. In studies like fraud, the cases that we would like to predict are usually rare. In our dataset, only %0.2 of transactions were fraud and my goal was to find a predictor to detect that small part of cases correctly. If I chose to use accuracy for model success metric then the grid search cv method would combine parameters to find a model, which may predict all cases as non-fraud. Because accuracy metric presents the percentage of correct predictions. If a model predicts all cases in our dataset as non-fraud then the accuracy score will be %99,8, which is quite impressive. However the model can not divide the observations as fraud or non-fraud.

After running the grid search cv algorithm on 4 different datasets, I found best parameter sets for logistic regression and decision tree classifiers and reached the precision and recall score combinations as below.


![PrecisionRecallTable](/images/Graph_2_Precision_recall_table.png)


When I compared models metrics and datasets with each other:
- The highest precision score ,%84 came from logistic regression and dataset-4 combination. If I specify extreme values, modify them on my dataset and mark them with flag features at the beginning, I am able to find a predictive model with the highest precision score. On the other hand the recall score was %62.
- The highest recall score,%78 was from decision tree and dataset-2. In this dataset, I only marked extreme values with flag features but I didn’t modify them. With this dataset and classifier combination, I got the precision score %80. 
- The dataset-3 had the worst results. Removing extreme values had a negative impact on predictive modelling. The variance between fraud and non-fraud cases were lost. 
![](/images/Graph_3_Classifiers_precision_recall_score.png)

In this study, I worked on the credit card transaction dataset to build a predictor for fraud case specification.

First I calculated descriptive statistics and made a histogram plot of each feature. This step gave me general information about features and pointed feature transformation requirements and extreme cases.

After this step, I handled extreme cases in different methods and created 4 datasets to work on. 

Finally, I built predictive models for each dataset and compared results to figure out which methodology led me to better results.

You can find my ipython notebook on Kaggle.
(https://www.kaggle.com/adilemrebilgic/credit-card-transactions-fraud-prediction)
