# amazon-reviews-dissatisfaction-prediction
## Problem Statement & Goal
Nowadays, customer satisfaction is one of the most important factors behind the success of any e-commerce company. That's why predicting customer dissatisfaction early, and acting on it before the customer leaves, is much better than waiting until they're already gone to react. With that in mind, I built an end-to-end project using NLP and machine learning that predicts whether a customer is satisfied or not, based only on their feedback (text).

## Dataset 
For this project, I used the Amazon Reviews 2023 dataset, focusing on the video games category. It's a huge dataset, with about 4.6 million reviews stored in around 2.5 GB of raw JSON.

## Data storage
To store the data, I used Amazon S3, which is well suited for large, semi-structured files like this. I created a bucket and uploaded my dataset to it.

<img width="452" height="253" alt="image" src="https://github.com/user-attachments/assets/f00f1cc5-1acc-4921-9ff2-d2e939d6d741" />


