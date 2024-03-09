# Customer Churn Prediction from Retail Order Data
Author: Giuseppe Cappella

This project exemplifies the end-to-end process of developing a churn prediction model starting from transaction data, from data preparation and feature engineering to model training, evaluation, and insight generation.

# Dataset description

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

# Project Structure and Methodology

The project is structured into two main parts:

## Part I : Data Preparation and Feature Engineering

The first part of the project tackles the challenge of working with order data that lacks a predefined response variable for churn. This is a common scenario when dealing with order datasets in Retail area, which typically consist of transactional information such as purchase dates and amounts spent but do not include explicit labels for customer churn. The primary task is, therefore, defining the target variable for churn.

### Key Steps:

**- Defining the Target Variable:** The analysis begins by excluding customers with fewer than two purchases. A repurchase curve is calculated at the 87th percentile, establishing a cutoff of 90 days to identify churn by assessing the presence of purchases in subsequent 90-day segments.

**- Data Segmentation and Feature Calculation:** The dataset is divided into 90-day periods, with analysis conducted to determine if purchases were made in the next segment. This method assigns a churn variable, indicating customers as churned if they did not make a purchase in the following 90 days.

**- Preparation for Modeling:** Additional features are developed, outliers are removed, and a labeled dataset indicating churn status is created, setting the stage for predictive modeling.

## Part II : Modeling and Evaluation**

Building upon the initial feature engineering efforts, the second part of the project focuses on the modeling, evaluation, and comparison of various machine learning models.

### Key Steps:

**- Preprocessing:** This includes encoding categorical variables, eliminating non-essential columns, and addressing missing and infinite values. The dataset is then split into training and test sets, preparing it for the modeling phase.

**- Predictive Modeling:** The BetaGeoFitter is used to calculate the predicted_purchases variable based on recency, T, and frequency. Models such as XGBoost, CatBoost, LightGBM, and a sequential neural network are trained and fine-tuned.

**- Model Evaluation:** The models are assessed using metrics like accuracy, confusion matrix, AUC, and other classification metrics, with ROC and Lift charts facilitating a direct performance comparison.

**- Performance Reporting:** A detailed performance report for the models is compiled using the kds library, offering a thorough comparison and understanding of each model's effectiveness.

**- Insight Generation:** A dive into the feature importance of the LightGBM model, which emerged as the top performer, is conducted. This analysis uncovers the critical factors influencing customer churn, providing actionable insights for crafting targeted retention strategies.
  
# Project results:

### The ROC curve has been utilized to compare the performance of our models.

The ROC curve, or Receiver Operating Characteristic curve, is a graphical plot that illustrates the diagnostic ability of a binary classifier system as its discrimination threshold is varied. It is created by plotting the True Positive Rate (TPR), also known as sensitivity, recall, or probability of detection, against the False Positive Rate (FPR), which is equal to 1 - specificity or the probability of false alarm, at various threshold settings. The corresponding Area Under the Curve (AUC) values offer insights into the effectiveness of each model. An AUC of 1.0 indicates a model that perfectly distinguishes between the two classes. An AUC of 0.5 suggests a model with no discriminative ability, equivalent to random guessing.

Below is a comparison of the ROC curves of the models.

<img width="765" alt="Schermata 2024-03-09 alle 12 33 38" src="https://github.com/peppecappella/customer-churn-prediction-project/assets/124899610/2fbe70a0-136e-4b14-b75f-33705b317bff">

The Lightgbm model outperforms the others in terms of AUC, whith a value of **0.81**, making it the leading choice for churn prediction in this specific case.

### In addition to the ROC curve, the lift chart has also been taken into consideration for comparing the performance of the models.

The lift chart is a tool used to evaluate the performance of predictive models, specifically how much better one can expect to do with the predictive model compared to without it.

The x-axis of the lift chart is divided into deciles, each representing a segment of the population. Typically, the first decile contains the individuals who are most likely to be true positives according to the model's predictions, and the last decile contains those who are least likely. The y-axis shows the lift, which is the ratio of the results obtained with the model to the results obtained without the model. A lift of 1 means no improvement over random selection.

Below is a comparison of Lift of the models:

<img width="740" alt="Schermata 2024-03-09 alle 12 34 45" src="https://github.com/peppecappella/customer-churn-prediction-project/assets/124899610/19aba156-f0aa-40b8-a7df-42fba809ff03">

From the chart, we see multiple lines, each representing a model and its corresponding lift across the deciles compared to a random selection baseline, which has a lift of 1 (no lift).Each model demonstrates varying degrees of lift. All the models start well above a lift of 1, indicating that they are all providing a better prediction than random chance, especially in the top deciles. Lightgbm stands out as the most effective, followed by Xgboost, Catboost, and lastly, the Neural Network. These insights are valuable when choosing a model for targeting interventions to prevent churn.

### Features importance

analysis of feature importance for the LightGBM model (the best performer) is performed, offering valuable insights into which attributes most significantly influence predictions. This process enables a deep understanding of the key drivers behind customer churn behavior, aiding in the development of targeted retention strategies.

Below is a chart bar showing for each features the related importance:

<img width="768" alt="Schermata 2024-03-09 alle 12 38 52" src="https://github.com/peppecappella/customer-churn-prediction-project/assets/124899610/e5b090aa-bb6b-48cb-81f5-44f1b07aa232">

### Features Explainability 

The project concludes with a section dedicated to feature explainability, a critical aspect of modern machine learning that ensures our models are transparent and their decisions understandable. By leveraging the SHAP (SHapley Additive exPlanations) framework, we have aimed to demystify the predictions made by our machine learning models. SHAP, grounded in cooperative game theory, assigns an importance value to each feature for a particular prediction, making it possible to understand the impact of each input variable on the model's output.

Specifically, we utilized the shap.force_plot function to visually dissect the contributions of each feature to individual predictions. 
For example, the command shap.force_plot(explainer.expected_value, shap_values[3,:], X_sampled.iloc[3,:]) was used to generate an interactive visualization illustrating how each feature of a sampled record influences the model’s prediction away from a baseline value.This baseline represents the prediction that would be made in the absence of any features. The SHAP values, derived for every feature, indicate whether the influence is positive or negative relative to this baseline, offering a detailed glimpse into the reasoning behind the model's predictions.

Below are, for example, shown the values for two distinct customers:

**Customer 1:**

![first_customer](https://github.com/peppecappella/customer-churn-prediction-project/assets/124899610/16dc1446-688b-4dcf-962c-6d141b0ffff5)

Customer 1 has a relatively low age and a high value of T which push the prediction towards a higher likelihood of churn. An important finding is that the presence of a value equal to 31 in the "favorite store" variable might suggest that this store is particularly problematic, and further analysis may be needed to determine if some stores have a higher concentration of customers who churn.

**Customer 2:**

<img width="955" alt="Schermata 2024-03-09 alle 18 45 55" src="https://github.com/peppecappella/customer-churn-prediction-project/assets/124899610/ffa73368-fa39-4353-b6ef-ca5dc89c4474">

For Customer 2, the plot shows a different story. Despite a high value of T, an age of 65 years together with consent to privacy push the prediction towards a lower likelihood of churn.

This level of granularity not only aids in the personalization of services and interventions but also in the identification of key factors that might influence customer behavior or outcomes. By visualizing the impact of each feature for different customers, we can uncover insights that would remain hidden in more aggregated forms of analysis, facilitating a deeper understanding of the diverse factors that drive the model's predictions.



