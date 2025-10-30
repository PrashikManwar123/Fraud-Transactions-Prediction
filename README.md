### 1. Problem Definition

**Objective:** The goal of this project is to **predict whether a financial transaction is fraudulent or genuine** using transaction-related features such as amount, type, and cardholder demographics. 

**Use Case:** Early detection of fraudulent activities enables banks and financial institutions to prevent suspicious transactions, safeguard customer funds, and enhance trust. By automating fraud detection through data-driven models, organizations can minimize losses and speed up response times.

### 2.  Dataset Overview

| Feature           | Description                                        |
| ----------------- | -------------------------------------------------- |
| `TransactionID`   | Unique ID for each transaction                     |
| `Amount`          | Transaction amount in USD                          |
| `TransactionType` | Mode of transaction — Online, ATM, or POS          |
| `IsInternational` | Whether the transaction was international (Yes/No) |
| `CardHolderAge`   | Age of the cardholder                              |
| `FraudReported`   | Target variable — "Yes" or "No"                    |


**Dataset size:** 50 transactions  
**Fraud cases reported:** 29 (58%)  
**Non-fraud cases:** 21 (42%)

**→ Insight:** The dataset is **slightly imbalanced**, with more fraudulent than non-fraudulent transactions, which is uncommon in real-world scenarios (where frauds are usually rare).

### 3. Descriptive Statistics 

![description](https://github.com/PrashikManwar123/Fraud-Transactions-Prediction/blob/main/description.png)

**→ Insight:** Transaction amounts vary widely (from ≈$30 to $990), showing a diverse range of spending behavior. Cardholders between **30–60 years** represent the majority of transactions.  
### 4.  Exploratory Data Analysis

1. No null values were found in the dataset, confirming that the data is clean and ready for processing.

2. The number of **fraudulent international transactions** is higher than fraudulent domestic ones. This aligns with real-world behavior — international transactions are riskier due to cross-border verification and currency exchange.

![description](https://github.com/PrashikManwar123/Fraud-Transactions-Prediction/blob/main/heatmap_int_fr.png)  

3. A scatter plot of `CardHolderAge` vs `Amount` shows that most cardholders fall in the **30–60 years** age range.) 
![description](https://github.com/PrashikManwar123/Fraud-Transactions-Prediction/blob/main/cardholderAge_vs_amount.png)

4. Age Group Analysis:

![description](https://github.com/PrashikManwar123/Fraud-Transactions-Prediction/blob/main/fraud_with_age.png)

 Three separate dataframes were created for:
 - <30 years
 - 30–60 years
 - >60 years
 
Fraud distribution within these groups shows that:

- Almost **90%** of people aged **below 30** and **above 60** have been victims of fraud.
- The **30–60** age group appears more cautious, with fewer fraud cases.

 4. Fraud and International Transaction Totals
![description](https://github.com/PrashikManwar123/Fraud-Transactions-Prediction/blob/main/total_no_fraud&int.png)

 Above graphs show total number of fraud, non-fraud, international and non-international transactions.

Fraud - 29
Not-fraud - 21
International - 27
Non-international(Domestic) - 23

**→ Insight:**  
Fraudulent and international transactions occur at a comparable scale, hinting at a potential overlap between the two.

7. Checking which transaction type is used by a particular age group
![description](https://github.com/PrashikManwar123/Fraud-Transactions-Prediction/blob/main/TransactionTypeByAgeGroup.png)

- People aged **30–60** prefer **ATM** and **Online** transactions.
    
- Those **under 30** or **over 60** show mixed transaction patterns, potentially leading to higher risk exposure.

8. Checking for fraud and non-fraud transactions in each transaction type
![description](https://github.com/PrashikManwar123/Fraud-Transactions-Prediction/blob/main/fraud&NonFraudByTransactionType.png)

Above graph shows that chances of fraud are highest in online and equal in POS and ATM type transactions.

**→ Insight:**  
Online transactions have the highest probability of fraud, likely due to weaker authentication mechanisms compared to in-person or ATM withdrawals.

9. Total Transaction Amount by Fraud Category

Category               Total Amount (USD)
Fraudulent             **15,709.61**
Non-Fraudulent         **11,440.59**

**→ Insight:**  
Fraudulent transactions make up **58% of the data** but account for **~58% of the total transaction value**, showing that fraudulent activity affects both count and monetary scale.

### 5. Data Transformation

1. To prepare the dataset for modeling, categorical columns were **label-encoded**: - ['TransactionType', 'IsInternational', 'FraudReported']

1 - Online
0 - ATM
2 - POS

2. Correlation of FraudReported with other columns
FraudReported - 1.000000 
TransactionType - 0.150461
CardHolderAge - 0.034161 
Amount - 0.004963 
IsInternational - 0.053661 

**→ Insight:**

The highest influence among these is TransactionType, meaning the type of transaction might be the most relevant factor — e.g., online or POS transactions could be more risky.

The weak overall correlations suggest that fraud behavior is nonlinear or multifactorial — likely depending on combinations of features (type + amount + international + age).

### 6. Splitting into train test

The dataset was divided into **training (70%)** and **testing (30%)** subsets to evaluate the model on unseen data. Stratified sampling was recommended due to the moderate class imbalance.

### 7. Model training

**Algorithm Used:** `DecisionTreeClassifier`

Reason for Choice:

- They handle categorical and numerical data effectively without needing feature scaling.
    
- Ideal for small datasets as they capture nonlinear relationships.

### 8. Model evaluation
| Metric    | Score |
| --------- | ----- |
| Accuracy  | 0.70  |
| Precision | 0.80  |
| Recall    | 0.67  |
| F1-Score  | 0.73  |
Precision (0.8) :  
The model correctly identifies fraudulent transactions with **80% precision**, meaning that 8 out of 10 flagged transactions are truly fraud. This is good for business — customers aren’t unnecessarily flagged or blocked.

Recall (0.67) : 
The model catches **about two-thirds of actual frauds** (67%). It _misses roughly one-third_ of real fraudulent transactions (false negatives). In fraud detection, **recall is critical** because missing a fraud can lead to financial loss.

F1-Score (0.73) : 
It combines both precision and recall into a single number. A score of 0.73 indicates **balanced performance** — decent accuracy and recall without too many false positives.

**Overall trade-off:**
The model is **conservative**, prioritizing correctness (precision) over completeness (recall).
This means it tends to only flag transactions when it’s _quite sure_ they’re fraud, avoiding false accusations — but some frauds may slip through.
### 9. Conclusion and Learning 

This project successfully demonstrates the development of a **machine learning pipeline for fraud transaction prediction** using a small but diverse dataset. 

The analysis revealed that **online transactions**, **high-value payments**, and **younger or senior cardholders** are more susceptible to fraud, while **transaction type** emerged as the most influential predictor. These findings can guide banks and payment systems in setting **dynamic verification rules** — for instance, triggering alerts for international or online transactions above certain thresholds.

The **Decision Tree Classifier** achieved an **accuracy of 70%**, **precision of 0.80**, **recall of 0.67**, and an **F1-score of 0.73**, indicating a balanced and moderately strong model. These results show that the model is **highly precise** in identifying fraudulent transactions — when it predicts fraud, it is correct most of the time — but it also **misses about one-third of actual frauds**, which is a common trade-off in fraud detection problems.

### 10. AI-Assisted Question–Answer Workflow

1. How can I identify which features are most influential in predicting fraud?
	**Implementation:**  
	Used the correlation matrix and `DecisionTreeClassifier.feature_importances_` to rank predictors.  
	
	**Result:**  
	`TransactionType` showed the highest influence (0.15 correlation with fraud), indicating that **online or POS** transactions are more likely to be fraudulent.

2. Is the dataset balanced, and how does imbalance affect model performance?
	**Implementation:**  
	`df['FraudReported'].value_counts()`  
	→ Fraud: 29 | Non-Fraud: 21  
	
	**Insight:**  
	Slight imbalance (58% fraud). Accuracy alone can mislead in such cases; need **precision** and **recall** metrics to judge the model fairly.

3. Does the cardholder’s age affect fraud likelihood?
	**Implementation:**  
	Split data into age groups (<30, 30–60, >60) and plotted fraud counts using bar charts.  
	**Insight:**  
	**Young (<30)** and **senior (>60)** users were targeted more frequently (~90% fraud in those groups), suggesting lower digital caution.

4. What model is suitable for a small dataset with mixed (categorical + numeric) data?
	**Implementation:**  
	Used `DecisionTreeClassifier` because it handles both data types, needs minimal preprocessing, and provides visual interpretability.  
	
	**Insight:**  
	The tree gave clear, rule-based decisions with decent accuracy (70%).
