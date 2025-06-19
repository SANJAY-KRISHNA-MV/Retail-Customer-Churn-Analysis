Phase 2: Data Acquisition & Understanding
Phase Overview
This phase focused on acquiring the raw transactional data, performing initial data loading, cleaning, and conducting an in-depth Exploratory Data Analysis (EDA). A crucial part of this phase was understanding the dataset's structure, identifying and handling data quality issues, and gaining preliminary insights that would inform subsequent feature engineering and modeling steps. The phase concluded with the definition of the churn observation window, a critical element for defining our target variable.

1. Data Acquisition
The project utilizes the "Online Retail II" dataset, obtained from Kaggle (https://www.kaggle.com/datasets/mashlyn/online-retail-ii-uci). This dataset provides transactional data from a UK-based online retail store over a period of approximately two years (December 2009 to December 2011).

The raw online_retail_II.xlsx file was downloaded and placed into the data/raw/ directory of the project repository.

2. Initial Data Loading and Inspection
The first step in the 01_data_acquisition_and_eda.ipynb notebook involved loading the .xlsx file into a Pandas DataFrame. Initial inspection was performed using:

df.info(): To check data types, non-null counts, and memory usage.

df.head(): To view the first few rows and understand the data structure.

df.describe(): To obtain descriptive statistics for numerical columns.

df.isnull().sum(): To identify the count of missing values per column.

df['Country'].value_counts(): To understand the distribution of geographical data.

Key observations from initial inspection:

Significant missing values in Customer ID, which are crucial for customer-level analysis.

Missing values in Description.

Presence of non-positive Quantity (returns) and Price values.

InvoiceNo contains entries prefixed with 'C', indicating cancelled orders.

InvoiceDate needed conversion to datetime objects for time-series analysis.

The actual unit price column was named Price, not UnitPrice.

3. Basic Data Cleaning
To ensure data quality and prepare the dataset for reliable feature engineering, the following cleaning steps were performed:

Missing Customer IDs: Rows with null Customer ID were dropped, as these transactions cannot be attributed to a specific customer for churn analysis.

Data Type Conversion: Customer ID was converted to an integer type. InvoiceDate was converted to a proper datetime format.

Missing Descriptions: Missing values in the Description column were filled with 'Unknown'.

Invalid Quantities/Prices: Rows where Quantity or Price were less than or equal to zero were removed. This filters out returns and invalid pricing entries, focusing on actual purchase behavior for churn analysis.

Cancelled Orders: Transactions with InvoiceNo starting with 'C' were identified and removed, as they represent cancelled orders rather than genuine purchases.

TotalPrice Calculation: A new column TotalPrice was calculated as Quantity * Price to represent the total cost of each item line.

Duplicate Rows: Exact duplicate rows were identified and removed.

The cleaned dataset was then saved as data/processed/online_retail_cleaned.csv for use in subsequent phases.

4. In-depth Exploratory Data Analysis (EDA)
After the initial cleaning, a deeper dive into the data characteristics was performed:

Time Range Analysis:

Determined the minimum and maximum InvoiceDate to understand the full temporal span of the dataset (approximately 2009-12-01 to 2011-12-09).

Visualized monthly unique transactions and total sales over time to observe trends and seasonality.

Distribution Analysis:

Histograms were generated for Quantity, Price, and TotalPrice (per item). These revealed highly skewed distributions, typical for retail transaction data, with most values concentrated at the lower end. Logarithmic scales were used for better visualization of these distributions.

Customer & Country Analysis:

Calculated the total number of unique customers.

Analyzed the top 10 countries by the number of transactions and total sales. This confirmed that the United Kingdom accounts for the vast majority of transactions and sales, which is expected for this dataset.

5. Churn Definition Strategy
A critical output of this phase was establishing the definition of "churn" for this project, given the absence of an explicit churn label in the dataset.

Analysis End Date: The latest transaction date in the dataset (2011-12-09) was set as the analysis_end_date.

Churn Window Duration: A 3-month duration was chosen for the churn observation window. This is a common period in retail for defining inactivity that leads to churn.

Observation End Date: This was defined as analysis_end_date - 3 months. All historical features (like RFM) will be calculated based on customer activity up to this observation_end_date.

Churn Labeling: A customer will be considered is_churned = 1 if their last purchase occurred before the observation_end_date AND they made no purchases during the subsequent 3-month churn window (from observation_end_date to analysis_end_date). Conversely, customers who had activity before the observation_end_date and did make a purchase within the churn window will be labeled is_churned = 0 (non-churned).

This completed Phase 2, providing a robust, cleaned dataset and a clear strategy for feature engineering and churn labeling.