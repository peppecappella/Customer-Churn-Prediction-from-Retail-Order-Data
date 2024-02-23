# customer-churn-prediction-project
Author: Giuseppe Cappella

This project exemplifies the end-to-end process of developing a churn prediction model starting from transaction data, from data preparation and feature engineering to model training, evaluation, and insight generation.
The project is structured into two main parts:

**Part I : Data Preparation and Feature Engineering**

The first part of the project tackles the challenge of working with order data that lacks a predefined response variable for churn. This is a common scenario when dealing with order datasets, which typically consist of transactional information such as purchase dates and amounts spent but do not include explicit labels for customer churn. The primary task is, therefore, defining the target variable for churn.

**Key Steps:**

- Defining the Target Variable: The analysis begins by excluding customers with fewer than two purchases. A repurchase curve is calculated at the 87th percentile, establishing a cutoff of 90 days to identify churn by assessing the presence of purchases in subsequent 90-day segments.

- Data Segmentation and Feature Calculation: The dataset is divided into 90-day periods, with analysis conducted to determine if purchases were made in the next segment. This method assigns a churn variable, indicating customers as churned if they did not make a purchase in the following 90 days.

- Preparation for Modeling: Additional features are developed, outliers are removed, and a labeled dataset indicating churn status is created, setting the stage for predictive modeling.

**Part II : Modeling and Evaluation**

Building upon the initial feature engineering efforts, the second part of the project focuses on the modeling, evaluation, and comparison of various machine learning models.

**Key Steps:**

- Preprocessing: This includes encoding categorical variables, eliminating non-essential columns, and addressing missing and infinite values. The dataset is then split into training and test sets, preparing it for the modeling phase.

- Predictive Modeling: The BetaGeoFitter is used to calculate the predicted_purchases variable based on recency, T, and frequency. Models such as XGBoost, CatBoost, LightGBM, and a sequential neural network are trained and fine-tuned.

- Model Evaluation: The models are assessed using metrics like accuracy, confusion matrix, AUC, and other classification metrics, with ROC and Lift charts facilitating a direct performance comparison.

- Performance Reporting: A detailed performance report for the models is compiled using the kds library, offering a thorough comparison and understanding of each model's effectiveness.

- Insight Generation: A dive into the feature importance of the LightGBM model, which emerged as the top performer, is conducted. This analysis uncovers the critical factors influencing customer churn, providing actionable insights for crafting targeted retention strategies.

# Additional Note on the Dataset

The initial dataset used in this project comprises a CSV file with 1,039,865 records, each record in the dataset represents an individual transaction.
Due to its size, the dataset is not included directly in this repository. However, a general description of the dataset is provided below to offer insights into the data structure and content.

- order_id: Uniquely identifies the transaction; all products purchased or refunded in the same transaction share this ID.
- customer_id: Identifies the customer, acting as a foreign key in the dataset.
- store_id: References the store where the transaction occurred, also a foreign key.
- product_id: Indicates the specific product purchased or refunded, serving as another foreign key.
- direction: Marks whether the product within the order has been purchased (1) or refunded (-1).
- gross_price: The total price of the product in Euros, which is the net price minus any price reduction. This value is negative if the product has been refunded.
- price_reduction: The discount applied to the product price in Euros, negative if the product has been refunded.
- purchase_datetime: The date and time when the purchase was made.


