Phase 3: Data Preprocessing & Feature Engineering

Phase Overview

This phase was dedicated to transforming the cleaned transactional data into a customer-level analytical dataset suitable for machine learning. The core activities involved defining the observation and churn periods, calculating essential customer behavioral features (like RFM), deriving customer tenure, assigning the churn target variable, and preparing the categorical features for modeling.

1. Data Loading and Period Definition

The process began by loading the online_retail_cleaned.csv dataset, which was the output of Phase 2, ensuring InvoiceDate was correctly parsed as datetime objects.

A critical step was the explicit definition of the time windows for churn analysis:

analysis_end_date: The maximum InvoiceDate present in the entire dataset. This marks the absolute end of our observation period (2011-12-09).

churn_window_duration: Set to 90 days (3 months), representing the period following the observation_end_date during which we monitor for customer activity to determine churn.

observation_end_date: Calculated as analysis_end_date - churn_window_duration. All features (e.g., RFM, tenure) are calculated based on customer activity up to this date. This prevents data leakage from the churn observation period into the features used for prediction.

Only transactions occurring on or before observation_end_date were used to compute the features, isolating the predictive power to historical behavior.

2. Feature Engineering: RFM Metrics

Recency, Frequency, and Monetary (RFM) analysis is a powerful technique for customer segmentation and behavior prediction. These metrics were calculated for each customer based solely on their activity within the observation_period.

Recency (R): The number of days since a customer's LastPurchaseDate relative to the observation_end_date. A lower Recency value indicates a more recently active customer.

Calculated by: (observation_end_date - max(InvoiceDate)).

Frequency (F): The total number of unique invoices (transactions) made by a customer within the observation_period. A higher Frequency indicates a more frequent shopper.

Calculated by: COUNT(DISTINCT InvoiceNo).

Monetary (M): The sum of TotalPrice for all purchases made by a customer within the observation_period. A higher Monetary value indicates a customer who spends more.

Calculated by: SUM(TotalPrice).

These three metrics were merged into a single customer_features DataFrame, with one row per unique Customer ID.

3. Feature Engineering: Customer Tenure

Customer tenure measures the loyalty or lifespan of a customer.

Tenure: The number of days since the customer's first ever purchase (their "registration" date) in the dataset up to the observation_end_date.

Calculated by: (observation_end_date - min(InvoiceDate) for each customer).
This feature provides context on how long a customer has been with the business, which can be a strong predictor of churn.

4. Churn Label Creation (Target Variable)

The is_churned target variable was created based on the defined churn window:

Customers who made at least one purchase within the observation_period were initially considered.

A customer was labeled is_churned = 1 if their last purchase date was before the observation_end_date AND they had no subsequent transactions within the churn_window (from observation_end_date to analysis_end_date).

A customer was labeled is_churned = 0 (non-churned) if they made at least one purchase within the churn_window.

The distribution of the churn label (churn rate) was inspected to understand the class imbalance, which is typical in churn prediction datasets.

5. Other Customer-Level Features & Categorical Encoding

Primary Country: The most frequent Country for each customer was identified and added as a feature, serving as a proxy for customer demographic/geographical information.

Categorical Feature Handling: The PrimaryCountry feature, being categorical, was prepared for machine learning models. To manage the potentially large number of unique countries, a strategy of grouping less frequent countries into an 'Other' category was applied (num_top_countries = 10).

One-Hot Encoding: The grouped PrimaryCountry_Grouped feature was then one-hot encoded using pd.get_dummies(). drop_first=True was used to avoid multicollinearity.

6. Output

The final DataFrame, df_final_features, containing all engineered features and the is_churned target variable, was saved to data/processed/customer_churn_features.csv. This dataset is now ready for the modeling phase.