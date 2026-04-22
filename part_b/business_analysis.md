# B1. Problem Formulation

## (a)

This problem can be solved using machine learning.

# Target Variable

The target variable is:

- **items_sold**

This means we want to predict how many items will be sold.

---

# Input Features

The input features can be:

- store_size  
- location_type  
- promotion_type  
- is_weekend  
- is_festival  
- competition_density  
- date-related features like month, day, etc.  

These features help the model understand the situation of each store.

---

# Type of Problem

This is a **regression problem**.

---

# Reason

It is a regression problem because we are predicting a number (items_sold).

If we were predicting categories like high or low sales, then it would be classification.

Also, this is supervised learning because we already have past data with inputs and output.

So the model can learn from past data and predict future sales.

## (b)

The company is using total sales revenue, but items_sold is a better choice.

Revenue can change because of price and discounts.  
For example, if there is a big discount, more items may be sold but revenue may not increase much.

So revenue does not clearly show how well a promotion is working.

items_sold is better because it directly shows how many products are sold.  
This makes it easier to understand the effect of promotions.

---

# Main Idea

This shows that in machine learning, we should choose the target variable carefully.

The target should clearly match what we want to measure.

If we choose the wrong target, the results may be confusing.

## (c)

Instead of using one single model for all stores, we can create separate models for different types of stores.

So using one model for all stores may not give accurate results.

Using separate models helps to better capture these differences and improve predictions.

This is better because stores in different areas behave differently.

So separate models will give better results.


______________________________________________________________________________________________

# B2. Data and EDA Strategy

## (a)

The data is given in different tables, so first I will combine them into one dataset.

# How I will join the tables

- I will join **transactions** with **store attributes** using store_id  
- Then I will join **promotion details** using promotion_type  
- Then I will join the **calendar table** using transaction_date  

This way, all the information will come into one table.

---

# Grain of the data

In the final dataset:

- one row will represent **one store on one day**

So each row will show how many items were sold in a store on a particular date.

---

# Aggregations before modelling

Before building the model, I will do some simple aggregations like:

- total items sold for each store per day  
- average sales for each store  
- total sales during different promotions  

This helps to organise the data properly and makes it easier for the model to learn.

---

# Simple idea

First, I combine all tables into one dataset.

Then, I make sure each row represents one store and one date.

Finally, I prepare the data by summarising it before using it in the model.

## (b)

Before building the model, I will do some basic EDA (Exploratory Data Analysis) to understand the data

1. Sales distribution (histogram)

plot a histogram of items_sold.

- This helps me see how sales are distributed (low, medium, high)
- If the data is very skewed, I may need to transform it (like log transformation)

---

2. Sales by promotion type (bar chart)

create a bar chart of average items_sold for each promotion type.

- This helps me see which promotion works better
- If some promotions give higher sales, it is an important feature

---

3. Sales by location type (bar chart)

compare sales for urban, semi-urban, and rural stores.

- This shows how location affects sales
- If there is a big difference, location_type becomes an important feature

---

4. Sales over time (line plot)

plot sales over time using transaction_date.

- This helps me see trends, seasonality, or spikes
- If I see patterns (like weekends or festivals), I can create new features

---

5. Correlation heatmap

 check correlation between numerical features.

- This helps to understand which features are related to sales
- Strong relationships can help in feature selection

---


EDA helps me understand the data better.

- Histogram → check sales distribution  
- Promotion chart → see which promotion is better  
- Location chart → compare stores  
- Time plot → see trends  

This helps choose better features and improve the model.

## (c)

Most of the data (80%) has no promotion.

# Problem


- The model will learn more from "no promotion" data  
- It may not learn properly how promotions affect sales  
- Predictions for promotion cases may be less accurate  

---

# Solution


- give more importance to promotion data  
- take a balanced sample of data  
- or check performance separately for promotion cases  

---

# Simple idea

If data is not balanced, the model may become biased.

So we should try to balance the data to get better results.

___________________________________________________________________________________________________________

# B3. Model Evaluation and Deployment

## (a)

# Train-Test Split

The data is time-based (monthly over 3 years), so I will not use random split.

Instead, I will:

- use the first part of the data (earlier months) as training data  
- use the most recent months as test data  

This way, the model learns from past data and predicts future data.

---
# Why random split is not good

Random split mixes past and future data.

This is a problem because:
- the model may learn from future information  

- results will not be realistic  

In real life, we always predict future using past data.

---
# Evaluation Metrics



# 1. RMSE (Root Mean Squared Error)

- shows overall error  
- gives more penalty to large mistakes  
- useful to understand big prediction errors  

# 2. MAE (Mean Absolute Error)

- shows average error  
- easy to understand  
- tells how far predictions are from actual values  

---

# Simple interpretation

- Lower RMSE → fewer big errors  
- Lower MAE → better average prediction  

Both help to understand how well the model is predicting items sold.

---
# conclution 

- train on past data  
- test on future data  
- use RMSE and MAE to check performance  

## (b)

The model is giving different recommendations for the same store in different months.

# investigate
check the feature importance from the model.

Feature importance shows which factors affect the prediction the most.

For example:
- month (December vs March)
- festival or not
- weekend or not
- promotion type
- store conditions

December may have festivals and higher demand, while March may have normal sales.

So the model uses these differences to recommend different promotions.

---
# explain to marketing team

- In December, there may be festivals and more customers  
- So Loyalty Points Bonus works better to increase engagement  

- In March, sales may be lower or normal  
- So Flat Discount helps to attract customers  

---
# conclution
Even for the same store, different months have different conditions.

The model looks at important features like time, festivals, and customer behavior.

That is why it gives different promotion recommendations.

## (c)

The model needs to give recommendations every month without training again.

# 1. Saving the model

After training, I will save the model using a file.

This can be done using tools like pickle or joblib.

This way, we don’t need to train the model again every time.

---

# 2. Using new monthly data

Every month:

- new data will come (store info, date, promotion details)
- So prepare the data in the same way (same preprocessing steps)
- then I will use the saved model to make predictions

This will give recommendations for each store.

---

# 3. Monitoring the model

Over time, the model performance may change.

So I will check:

- prediction errors (RMSE or MAE)
- compare predicted vs actual sales

If the error becomes high, it means the model is not working well.

---

# 4. Retraining

If performance becomes worse:

- I will retrain the model using new data  
- then save the updated model  

---

# conclution
- train once and save the model  
- use it every month for prediction  
- keep checking performance  
- retrain when needed  