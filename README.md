# Customer Segmentation for Personalized Banking

## Project Overview

This project aims to perform customer segmentation using transaction data to enable a personalized banking system. By understanding distinct customer groups, the bank can tailor its products, services, and marketing strategies to meet specific customer needs, ultimately enhancing customer satisfaction, retention, and profitability.

## Data Source

The analysis is based on the `bank_transactions.csv` dataset, which contains various transaction-related information and customer demographics.

## Methodology

### 1. Data Loading and Initial Inspection

The `bank_transactions.csv` file was loaded into a pandas DataFrame. Initial inspection using `df.info()` and `df.isnull().sum()` revealed missing values in `CustomerDOB`, `CustGender`, `CustLocation`, and `CustAccountBalance`. No duplicate rows were found in the dataset.

### 2. Data Cleaning and Preparation

*   **Age Calculation and Correction**: 'CustomerDOB' was used to calculate 'Age'. Negative age values (resulting from incorrect date parsing) were corrected by adding 100 years. Ages less than 18 were replaced with the median age, assuming banking customers are adults.
*   **Missing Value Imputation**: Missing values in `CustGender` and `CustLocation` were imputed using the mode. Missing values in `CustAccountBalance` and `Age` (after initial calculation) were imputed using the median.
*   **Data Type Conversion**: `TransactionDate` was converted to datetime objects for temporal analysis.

### 3. Exploratory Data Analysis (EDA)

*   **Numerical Features**: Distributions of numerical columns (`CustAccountBalance`, `TransactionTime`, `TransactionAmount (INR)`, `Age`) were visualized using histograms. The correlation matrix was plotted to understand relationships between numerical features.
*   **Categorical Features**: Value counts and count plots were generated for `CustGender` and `CustLocation` (focusing on top 10 locations).
*   **Feature Relationships**: Box plots were used to examine transaction amounts across different genders and top 10 customer locations.
*   **Temporal Trends**: Daily total transaction amounts and transaction counts over time were plotted to identify trends.

### 4. Feature Engineering

New features were engineered to capture more granular customer behavior:

*   **Temporal Features**: `TransactionHour`, `TransactionMinute`, `TransactionSecond`, `TransactionDayOfWeek`, `TransactionMonth`, `TransactionDay` were extracted from `TransactionTime` and `TransactionDate`.
*   **Customer-Level Aggregations**: Features were aggregated per `CustomerID` to create a customer-centric dataset (`customer_features`). These included:
    *   `NumTransactions`: Total number of transactions.
    *   `TotalTransactionAmount`: Sum of all transaction amounts.
    *   `AverageTransactionAmount`: Average transaction amount.
    *   `MaxTransactionAmount`: Maximum transaction amount.
    *   `MinTransactionAmount`: Minimum transaction amount.
    *   `CustomerAge`: Age of the customer.
    *   `CustomerGender`: Most frequent gender.
    *   `CustomerBalance`: Latest account balance.

### 5. Feature Scaling

Numerical features in the `customer_features` DataFrame were scaled using `StandardScaler`. This step is crucial for distance-based clustering algorithms like K-Means, ensuring that features with larger magnitudes do not disproportionately influence the clustering results.

### 6. Determining Optimal Number of Clusters (K)

*   **Elbow Method**: The Within-Cluster Sum of Squares (WCSS) was calculated for K-Means with 1 to 10 clusters. The Elbow Method plot indicated an optimal K around **3 or 4**.
*   **Silhouette Score (Attempted)**: The Silhouette Score was attempted but proved too computationally intensive for the large dataset, even with sampling. Given the clear indication from the Elbow Method, the project proceeded with K=4.

### 7. K-Means Clustering

The K-Means algorithm was applied to the scaled customer features with **K=4**. Each customer was assigned a cluster label, segmenting the customer base into four distinct groups.

### 8. Profiling Customer Segments

Each of the four clusters was profiled by calculating the mean of the original (unscaled) numerical features and the distribution of `CustomerGender` within each cluster. This provided insights into the unique characteristics and behaviors of customers in each segment:

*   **Cluster 0: "Everyday Users" (Largest Segment)**: Characterized by a single transaction, moderate transaction amounts and account balances. Predominantly male.
*   **Cluster 1: "High-Net-Worth Individuals (HNWIs)" (Smallest, but Highest Value Segment)**: Extremely high transaction amounts, average transaction amounts, max/min transaction amounts, and account balances. Slightly older, mostly male.
*   **Cluster 2: "Affluent Customers" (Mid-Size High-Value Segment)**: High transaction values and balances, more transactions than Everyday Users, older. Mostly male.
*   **Cluster 3: "Active Engaged Transactors" (Second Largest Segment)**: High transaction frequency with moderate individual amounts, indicating regular and active banking usage. More balanced gender distribution.

### 9. Business Recommendations

Based on the customer segment profiles, specific personalized banking strategies were recommended:

*   **Cluster 0 (Everyday Users)**: Focus on engagement, product upselling (savings, small loans), digital adoption, and financial literacy.
*   **Cluster 1 (HNWIs)**: Implement dedicated relationship management, offer exclusive products (investments, private banking), premium benefits, and emphasize security/privacy.
*   **Cluster 2 (Affluent Customers)**: Provide tiered premium services, targeted investment products, financial advisory, and customized credit solutions.
*   **Cluster 3 (Active Engaged Transactors)**: Introduce rewards for activity, ensure convenience/efficiency, offer budgeting tools, and cross-sell complementary products (credit cards, micro-investments).

This segmentation allows the bank to develop targeted and effective strategies to better serve its diverse customer base.
