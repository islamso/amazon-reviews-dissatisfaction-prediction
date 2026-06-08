# Predicting Customer Dissatisfaction from Amazon Video Game Reviews

## Problem Statement & Goal
Nowadays, customer satisfaction is one of the most important factors behind the success of any e-commerce company. That's why predicting customer dissatisfaction early, and acting on it before the customer leaves, is much better than waiting until they're already gone to react. With that in mind, I built an end-to-end project using NLP and machine learning that predicts whether a customer is satisfied or not, based only on their feedback (text).

## Dataset 
For this project, I used the Amazon Reviews 2023 dataset, focusing on the video games category. It's a huge dataset, with about 4.6 million reviews stored in around 2.5 GB of raw JSON.

## Data storage
To store the data, I used Amazon S3, which is well suited for large, semi-structured files like this. I created a bucket and uploaded my dataset to it.

<img width="452" height="253" alt="image" src="https://github.com/user-attachments/assets/f00f1cc5-1acc-4921-9ff2-d2e939d6d741" />

## Data cleaning
Next, I cleaned the data using PySpark on Databricks, which can handle millions of rows efficiently. I removed duplicate reviews and filtered out very short, low-value ones (under 20 characters), which left about 4 million high-quality reviews. I then saved the cleaned data back to S3 as Parquet, which is smaller and much faster to read than JSON.

## sampling 
After cleaning, I took a stratified sample of about 300,000 reviews instead of working with the full dataset. Sampling keeps the same proportion of each star rating, so the data stays balanced, while making training much faster  minutes instead of hours. This smaller sample was then moved into Pandas, which is better suited than Spark for the modelling.

## Exploratory data analysis
Before modelling, I ran some exploratory data analysis (EDA) to understand the dataset and how the ratings were spread out. The ratings followed a clear J-shape: about 58% were five stars and 17% were one star, with very few in between. This matters, because such an unbalanced dataset can trick a model into always predicting "positive", so I kept this in mind during modelling.

<img width="452" height="220" alt="image" src="https://github.com/user-attachments/assets/9d442572-aca4-4aab-9e93-752a14cb36b4" />




