# Predicting Customer Dissatisfaction from Amazon Video Game Reviews

## Problem Statement & Goal
Nowadays, customer satisfaction is one of the most important factors behind the success of any e-commerce company. That's why predicting customer dissatisfaction early, and acting on it before the customer leaves, is much better than waiting until they're already gone to react. With that in mind, I built an end-to-end project using NLP and machine learning that predicts whether a customer is satisfied or not, based only on their feedback (text).

## Dataset 
For this project, I used the Amazon Reviews 2023 dataset, focusing on the video games category. It's a huge dataset, with about 4.6 million reviews stored in around 2.5 GB of raw JSON.

## Data storage
To store the data, I used Amazon S3, which is well suited for large, semi-structured files like this. I created a bucket and uploaded my dataset to it.

<img width="452" height="253" alt="image" src="https://github.com/user-attachments/assets/f00f1cc5-1acc-4921-9ff2-d2e939d6d741" />

## Data pre processing
Next, I cleaned the data using PySpark on Databricks, which can handle millions of rows efficiently. I removed duplicate reviews and filtered out very short, low-value ones (under 20 characters), which left about 4 million high-quality reviews. I then saved the cleaned data back to S3 as Parquet, which is smaller and much faster to read than JSON.

## sampling 
After cleaning, I took a stratified sample of about 300,000 reviews instead of working with the full dataset. Sampling keeps the same proportion of each star rating, so the data stays balanced, while making training much faster  minutes instead of hours. This smaller sample was then moved into Pandas, which is better suited than Spark for the modelling.

## Exploratory data analysis
Before modelling, I ran some exploratory data analysis (EDA) to understand the dataset and how the ratings were spread out. The ratings followed a clear J-shape: about 58% were five stars and 17% were one star, with very few in between. This matters, because such an unbalanced dataset can trick a model into always predicting "positive", so I kept this in mind during modelling.

<img width="452" height="220" alt="image" src="https://github.com/user-attachments/assets/9d442572-aca4-4aab-9e93-752a14cb36b4" />

I also checked the dataset structure and performed sentiment analysis using VADER to understand how the sentiment scores related to the star ratings.

## Modelling 

For the modelling step, I trained and compared four machine learning models: Logistic Regression, Naive Bayes, Linear SVM, and Random Forest. I turned the reviews into numbers using TF-IDF, and used only the review text as input  not the star rating so the model had to learn from the words alone. Each model was trained twice, once normally and once with balanced class weights, to deal with the imbalance I found during EDA.

Logistic Regression was the best model, with an Recall of 90%, 0.88 F1 score and  and an AUC of 0.969.

<img width="452" height="116" alt="image" src="https://github.com/user-attachments/assets/b357016d-d82d-4c63-b151-87f18ca275cd" />

These results show how good the model was at detecting unsatisfied customers and The AUC of 0.969 shows the model separates satisfied and unsatisfied customers very well.

## Cost & Security
To keep costs low, I stored the cleaned data as Parquet, which uses far less space than JSON, and used Databricks serverless compute, which only charges while a job is running. Sampling the data down to 300k rows also cut training time from hours to minutes. As a safety net, I set up a $1 AWS budget alert to warn me at the first sign of unexpected charges.
I also created a dedicated IAM user with only the permissions it needed, and kept the root account for setup only, with MFA enabled. No AWS keys were ever written inside the notebooks, instead, Databricks connected to S3 through a Unity Catalog role. On the bucket itself, I turned on public access blocking, encryption, and versioning.

## Limitations and next step
The model uses a bag-of-words approach (TF-IDF), so it can miss sarcasm, hedging, and gaming-specific words like "brutal" or "addictive" that don't mean what they usually do. The data upload to S3 was also done manually, which is fine for a prototype but not for production. The most promising next step would be to use a transformer-based model, which understands context much better and should improve the results. For a real deployment, I would also automate the data ingestion, add CloudWatch monitoring and scheduled retraining, and tighten the IAM permissions to be even more strict.

## tech stack 

PySpark: large-scale data processing
scikit-learn : TF-IDF and the ML models
vaderSentiment : sentiment scoring
Pandas / NumPy : data handling on the sample
matplotlib / seaborn : charts and visualisation

## Author 
Built by Islam soussi, an AI Engineer.


