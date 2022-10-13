# Telco-Customer-Churn
## Background ##
After the interruption of Fido few months ago, I noticed that there are a lot of people talk about choosing another telco with better and more stable service. So I began to think about the churn of Telco Customer.

Especially nowadays, the telecom market is gradually becoming saturated, everyone has at least one telecom account. For the development of the telecom industry, the user retention rate is particularly important. 

## Data Resource ##
The data for this case comes from the modeling dataset which is shared on Kaggle: Telco Customer Churn. in the dataset, there are more than 7000 rows and 21 columns, each row represents a customer, each column contains customer’s attributes, such as their age, gender, whether they have phone service or multiple lines, etc.

## Target ##
- Analyze the relationship between user characteristics and churn.
- From the overall situation, what are the common characteristics of lost users?
- Provide targeted suggestions to increase user stickiness and prevent loss.

## Data Cleaning ##
After getting the data, the first step is data cleaning, which mainly includes removing duplicate values, removing outliers, filling nulls and converting data types.
- For duplicate data, since each user has a unique code, it is sufficient to check for duplicate user codes.
- For outliers, check if there are values that are not at the mean plus or minus three times the standard deviation, and if so, they are considered to be outliers. 
- For null values, simply using pd.isnull() can not ensure that all null values will be found, as there may be cases where the null value is represented by a symbol such as " " or "-". So I first converted the data from "object" to "float64", found the line that reported the error, and then checked it and found that there was indeed “totalcharge” were missing value and represented by " ". 
- For the existence of empty value of the line, I sorted out and found that their tenure are 0, that is to say, these users are new users of the month of statistics, for the analysis of user churn is not very meaningful, and accounted for very little, only 11 users, so directly delete these data.
- When converting data types, the discrete data is mainly converted to 0 and 1 by pd.get_dummies().

## Analysis process ##
The correlation between the different dimensions is calculated by .corr().
According to the available data, there are 19 dimensions for each user to analyze, which can be roughly divided into three categories: personal information, account information, and product information.
By ranking the correlation coefficients, it can be seen that there are some dimensions that have no significant effect on user churn under different data categories 

First, it can be seen that in the personal information category
- gender has almost no impact on user churn, while age has a greater impact. 

In the account information category, tenure, contract, payment method and fees are more influential:
- sticky the users are more likely to have the longer the tenure, the longer the contract, the lower the charges
- comparing with other payment method, the electronic check is the last popular than other payment method.

For the service category, online security, tech support, internet service - fiber optics, online back up, device protection are more important than others features.
- when service can include online security, tech support, online back up and device protection, customers have higher loyalty.
- customers with fiber optics service are more likely to churn than they have other internet service.
- Also, it seems that customers who do not have internet servce have less possiblity to churn. 


## Predict ##
Since there are a total of 19 features that may affect user stickiness, the random forest model is chosen here for analysis. Since there is only one table and no data for training and testing the model, the data is splited into training and testing classes by train_test_split(X, y,random_state = 0), fit the model by .fit(train_X, train_y). finally check the reliablity by .score(train_X, train_y). seems that this is a good model to predict the churn of customers.

## Suggestion to increase user stickiness ##
对于不同画像的用户有不用的维系方式，比如对于老人提高对tech的支持以及减少捆绑销售，对于年轻用户重点推送套餐，以及合同等
