Retail Customer Churn Analysis
Project Overview
This project focuses on building a machine learning model to predict customer churn for a retail business, identifying key factors influencing churn, and translating these insights into actionable retention strategies. Leveraging a real-world transactional dataset (Online Retail II from Kaggle), this project demonstrates a comprehensive data science workflow from data acquisition and cleaning to feature engineering, advanced machine learning modeling (Gradient Boosting), and interactive visualization using Streamlit.

The primary objective is to empower business stakeholders with a data-driven tool to proactively identify customers at high risk of churning and understand why they are churning, enabling targeted interventions to improve customer retention and lifetime value.

Problem Statement
Customer churn is a significant challenge for retail businesses, directly impacting revenue and growth. Identifying at-risk customers early allows businesses to implement targeted retention campaigns (e.g., personalized offers, improved customer service) before customers are lost. This project addresses this by:

Developing a predictive model for customer churn.

Uncovering the most influential behavioral and transactional factors that contribute to churn.

Providing an interactive platform for stakeholders to explore churn insights and identify at-risk customers.

Dataset
This project utilizes the Online Retail II Dataset from Kaggle.

Source: https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci

Description: A transactional dataset containing all purchases made by customers of a UK-based online retail store. It spans a period from December 2009 to December 2011.

Key Columns: InvoiceNo, StockCode, Description, Quantity, InvoiceDate, Price, CustomerID, Country.

Note on Churn Definition: As the dataset does not contain an explicit churn label, churn was defined based on customer inactivity. A customer was considered churned if they had no transactions within a 3-month churn observation window following their last activity in the analysis period.

Project Phases & Methodology
The project followed a structured data science methodology:

Project Planning & Setup: Defined project scope, objectives, initial tools, and established the repository structure.

Data Acquisition & Understanding: Acquired the Online Retail II dataset, performed initial data loading, cleaning (handling missing Customer IDs, invalid quantities/prices, cancelled orders), and comprehensive Exploratory Data Analysis (EDA) to understand data distributions and temporal trends.

Data Preprocessing & Feature Engineering:

Defined a 3-month churn window and an observation period.

Engineered crucial customer-level features:

RFM (Recency, Frequency, Monetary): Calculated based on customer activity within the observation period.

Tenure: Days since the customer's first purchase.

Prepared the Country feature (grouping top countries, one-hot encoding).

Created the binary is_churned target variable.

Modeling & Evaluation:

Split the feature-engineered data into training and testing sets, stratifying by churn status.

Trained and evaluated multiple classification models (Logistic Regression, Random Forest, Gradient Boosting).

Selected Gradient Boosting Classifier for its superior initial performance (highest ROC-AUC).

Performed hyperparameter tuning (GridSearchCV) on the Gradient Boosting model to optimize its performance, primarily focusing on maximizing ROC-AUC.

Evaluated the final model using a suite of metrics: Accuracy, Precision, Recall, F1-Score, ROC-AUC, and Confusion Matrix.

Identified Feature Importances from the best model to understand key churn drivers.

Visualization & Communication:

Developed an interactive Streamlit dashboard to visualize churn insights.

The dashboard displays overall churn rates, churn by country, top churn drivers, and allows users to identify at-risk customers based on a adjustable probability threshold.

Technologies Used
Programming Language: Python 3.x

Data Manipulation & Analysis: pandas, numpy

Machine Learning: scikit-learn (Logistic Regression, RandomForestClassifier, GradientBoostingClassifier, GridSearchCV, train_test_split, metrics)

Model Persistence: joblib

Visualization: matplotlib, seaborn, plotly, streamlit (for interactive dashboard)

Version Control: Git, GitHub

Development Environment: Jupyter Notebook

Results & Insights (Example - UPDATE WITH YOUR ACTUAL VALUES)
The Tuned Gradient Boosting Classifier was chosen as the final model due to its robust performance, achieving excellent discriminative power for churn prediction.

Key Model Performance on Test Set:

Accuracy: [Your Accuracy Value]

Precision: [Your Precision Value]

Recall: [Your Recall Value]

F1-Score: [Your F1-Score Value]

ROC-AUC: [Your ROC-AUC Value]

Confusion Matrix: [[TN, FP], [FN, TP]] (e.g., [[369, 204], [124, 624]])

Top Churn Drivers (from Gradient Boosting Feature Importance):
(Based on your notebook's output, list the top 3-5 most important features, e.g.:)

Recency: (e.g., 0.45 importance) - Customers who haven't purchased recently are at higher risk.

Monetary: (e.g., 0.20 importance) - Lower spending customers might be more prone to churn.

Frequency: (e.g., 0.15 importance) - Less frequent buyers have higher churn probability.

Tenure: (e.g., 0.08 importance) - Newer customers might churn faster, or very old customers might become dormant.

Country_Germany: (e.g., 0.03 importance) - Specific country dynamics can influence churn.

Actionable Business Recommendations:

Targeted Re-engagement Campaigns: Focus on customers with high Recency (those who haven't purchased in a while). Personalized offers or loyalty program reminders can encourage return visits.

Value-Based Retention: Implement strategies for low Monetary value customers, such as bundled offers or loyalty discounts to increase their average basket size.

Onboarding for New Customers: Pay close attention to customers with low Tenure (newer customers) who show early signs of churn by ensuring positive initial experiences.

Geographic Specific Campaigns: Analyze churn patterns in specific countries (e.g., Country_Germany) to tailor retention efforts to regional preferences or market conditions.

Proactive Customer Service: Identify predicted at-risk customers from the dashboard and initiate proactive outreach from customer service to address potential issues.

How to Run the Project
To set up and run this project locally, follow these steps:

Clone the Repository:

git clone https://github.com/YourUsername/Retail-Customer-Churn-Analysis.git
cd Retail-Customer-Churn-Analysis

Create a Virtual Environment (Recommended):

python -m venv venv
# On Windows:
# .\venv\Scripts\activate
# On macOS/Linux:
# source venv/bin/activate

Install Dependencies:

pip install -r requirements.txt

Download Raw Data:

Go to the Kaggle dataset: Online Retail II UCI

Download the online_retail_II.zip file.

Extract online_retail_II.xlsx and place it in the data/raw/ directory.

Run Jupyter Notebooks (Sequential Order):

Launch Jupyter Notebook or JupyterLab from the project root directory:

jupyter notebook

Navigate to the notebooks/ folder.

Run the notebooks in the following order to generate processed data and train the model:

01_data_acquisition_and_eda.ipynb

02_feature_engineering.ipynb

03_model_training_and_evaluation.ipynb

04_dashboard_insights.ipynb (This will create customer_churn_predictions.csv)

Run the Streamlit Dashboard:

Open your terminal in the project's root directory.

Execute the Streamlit application:

streamlit run src/visualization_app.py

This will open the interactive dashboard in your web browser.

Project Structure
Retail-Customer-Churn-Analysis/
├── .gitignore               # Specifies intentionally untracked files to ignore.
├── README.md                # This file: Project description, setup, usage.
├── requirements.txt         # Lists all Python package dependencies.
├── notebooks/               # Jupyter notebooks documenting the project workflow.
│   ├── 01_data_acquisition_and_eda.ipynb
│   ├── 02_feature_engineering.ipynb
│   ├── 03_model_training_and_evaluation.ipynb
│   └── 04_dashboard_insights.ipynb
├── src/                     # Modular Python scripts for reusable functions and Streamlit app.
│   └── visualization_app.py # The Streamlit interactive dashboard.
├── models/                  # Directory to store trained machine learning models (e.g., best_churn_model.joblib).
├── data/                    # Directory for storing raw and processed datasets.
│   ├── raw/                 # Contains the original 'online_retail_II.xlsx' dataset.
│   └── processed/           # Contains cleaned, feature-engineered, and predicted datasets.
├── documentation/           # Detailed documentation for each project phase.
│   ├── 01_project_planning_setup.md
│   ├── 02_data_acquisition_and_understanding.md
│   ├── 03_data_preprocessing_and_feature_engineering.md
│   ├── 04_modeling_and_evaluation.md
│   └── 05_visualization_and_communication.md
└── LICENSE                  # (Optional) Defines the licensing for the project.

Future Work / Stretch Goals
Advanced Feature Engineering: Explore more complex features such as product categories purchased, average time between purchases, or sentiment from customer reviews (if available).

More Sophisticated Modeling: Experiment with other advanced models like XGBoost, LightGBM, or CatBoost.

Model Deployment: Simulate deploying the model as an API endpoint (e.g., using Flask/FastAPI) for real-time predictions.

Customer Segmentation: Beyond churn prediction, perform deeper customer segmentation (e.g., using clustering) to tailor retention strategies more precisely.

A/B Testing Framework: Propose a framework for A/B testing different retention strategies derived from the model's insights.

Time Series Forecasting: Incorporate time-series analysis on customer activity to predict future engagement levels.

Streamlit Enhancements: Add more interactive elements, filtering options, or detailed views for specific customer segments.

Contact
Your Name: [Your Name Here]

GitHub: [Link to your GitHub profile]

LinkedIn: [Link to your LinkedIn profile (Optional)]

Email: [Your Email Address (Optional)]