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

<p align="center">
  <img src="https://github.com/Yuxuanjojo/Predict_future_sales/blob/main/img_predict/dataset_review1.png?raw=true" width="400px">    
</p>  

- For null values, simply using pd.isnull() can not ensure that all null values will be found, as there may be cases where the null value is represented by a symbol such as " " or "-". So I first converted the data from "object" to "float64", found the line that reported the error, and then checked it and found that there was indeed “totalcharge” were missing value and represented by " ". 

<p align="center">
  <img src="https://github.com/Yuxuanjojo/Predict_future_sales/blob/main/img_predict/dataset_review1.png?raw=true" width="400px">    
</p>  
<p align="center">
  <img src="https://github.com/Yuxuanjojo/Predict_future_sales/blob/main/img_predict/dataset_review1.png?raw=true" width="400px">    
</p>  

- For the existence of empty value of the line, I sorted out and found that their tenure are 0, that is to say, these users are new users of the month of statistics, for the analysis of user churn is not very meaningful, and accounted for very little, only 11 users, so directly delete these data.

<p align="center">
  <img src="https://github.com/Yuxuanjojo/Predict_future_sales/blob/main/img_predict/dataset_review1.png?raw=true" width="400px">    
</p>  

- When converting data types, the discrete data is mainly converted to 0 and 1 by pd.get_dummies().

## Analysis process ##
The correlation between the different dimensions is calculated by .corr(). According to the available data, there are 19 dimensions for each user to analyze, which can be roughly divided into three categories: personal information, account information, and product information. By ranking the correlation coefficients, it can be seen that there are some dimensions that have no significant effect on user churn under different data categories.

First, it can be seen that in the personal information category
- gender has almost no impact on user churn, while age has a greater impact. 

In the account information category, tenure, contract, payment method and fees are more influential:
- Sticky the users are more likely to have the longer the tenure, the longer the contract and the lower the charges.
- Compared with other payment methods, the electronic check is the last popular payment method.

For the service category, online security, tech support, internet service 
- fiber optics, online back up, device protection are more important than others features.
- When services can include online security, tech support, online backup and device protection, customers have higher loyalty.
customers with fiber optics service are more likely to churn than they have other internet service.
- Also, it seems that customers who do not have internet service have less possibility to churn.
- Users without internet service are less likely to churn
- StreamTV and Streammovie effect slightly to customers churn


## Predict ##
Since there are a total of 19 features that may affect user stickiness, the random forest model is chosen here for analysis. Since there is only one table and no data for training and testing the model, the data is splited into training and testing classes by train_test_split(X, y,random_state = 0), fit the model by .fit(train_X, train_y). finally check the reliablity by .score(train_X, train_y). seems that this is a good model to predict the churn of customers.

## Suggestion to increase user stickiness ##
According to the above analysis, it can be seen that young people and economically independent people have higher loyalty, and users with long contract time and low monthly charge have higher loyalty. Few users without internet services have higher loyalty, while users with internet services are very highly regarded for online security, tech support, online backup and device protection. Especially for senior customers without partners, technical support is the main factor leading to the loss of users. On another hand, users with fiber have a higher possibility to churn.

Therefore, for young users with families, they can focus on increasing package sales of network services and focus on improving services related to network services, such as network security, online backup, and device protection. For the older group, they should focus more on the economics of phone service, and if the customer's service includes network service, they should focus on improving technical support as well as device protection, meantime, for the older user groups, bundled sales can be reduced appropriately. For users who are not independent, device security as well as network security is also very important.

In addition, users who have used fiber optics have a higher churn rate than other internet services, so there may be some problems with the stability or speed of the fiber optic network or other aspects. thus the technical department can pay more attention to that. Although the data shows that the longer the contract time may cause the lower the churn rate, the sales process should not simply lengthen the contract time, because users may be discouraged by too long a service contract.
