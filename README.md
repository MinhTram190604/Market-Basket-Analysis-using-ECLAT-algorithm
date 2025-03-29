# Market Basket Analysis using ECLAT algorithm

## Description:
This project use  ECLAT algorithm to discover frequent itemsets and strong association rules from transaction data in order to analyze customer purchasing behavior. The discovery  can be applied in practice to provide actionable recommendations for business, such as build cross-selling strategy or optimize product placement strategy.

## Techniques Used:
- Python programming language (pandas, numpy, collections, itertools, matplotlib, wordcloud, pyECLAT).
- Preprocessing data techniques (convert dataset to vertical format for ECLAT implementation.)
- ECLAT algorithm and evaluating metrics (support, confidence, LIFT)

## Step 1: EDA
- The dataset has 999 rows (each row is a transaction) and 16 columns (16 items).
- There are no columns with missing values.
- All columns have the data type bool, where True indicates that the transaction contains the item, and False means it does not.

![image](https://github.com/user-attachments/assets/c6acb9b2-8b14-4a4d-9f97-dc176b7f575f)  
The products appear with nearly the same frequency.  
![image](https://github.com/user-attachments/assets/08cff288-e20d-483f-8b21-0728f8b60a05)  
- According to the descriptive statistics, the range of the number of transactions per product is 38.
- Each product appears in approximately 383 to 421 transactions out of a total of 999 transactions.
- The products appear in about 38% to 42% of all transactions, which is considered quite high. 

![image](https://github.com/user-attachments/assets/6eca0dab-9c98-4edf-a5ec-7da6b1d1b876)  
- Transactions with 7 products are the most frequent, followed by transactions with 8 and 9 products.
- Starting from 11 products, the number of transactions decreases, with the lowest number observed at 13 products.
- There are no transactions containing 14 or more products.


## Step 2: Identifying Frequent Itemsets
### 2.1 Choosing an appropriate Minimum Support Threshold
The minimum support threshold is determined based on the average frequency of product pairs. Specifically, the algorithm:
- Counts the occurrence of all possible product pairs in the transaction dataset.
- Calculates the average support of these product pairs.
- Multiplies the average support by a target ratio to adjust the min support threshold according to the analytical purpose.  
The target ratio is set to 0.6, meaning the focus is on product pairs with relatively high support values.
The min support is 0.11.

### 2.2 Implementing the ECLAT algorithm using the pyECLAT library
https://pypi.org/project/pyECLAT/
The eclat_instance of pyECLAT library provides two methods: fit() and fit_all(). 

The author chooses to use the fit_all() method because the goal is to analyze the entire dataset in order to gain deeper insights into all frequent itemsets.  

The fit_all() method returns a dictionary where:
- Key: The name of the itemset (a combination of products)
- Value: The support value (the frequency of occurrence of that itemset across all transactions)

## Step 3: Generating Association Rules
### 3.1 Calculating Confidence for Frequent Itemsets
The Confidence of an association rule is computed based on the frequency of the entire itemset (LHS ∪ RHS) compared to the frequency of the left-hand side (LHS) only.
- The algorithm iterates through all frequent itemsets of size ≥ 2.
- For each itemset, it splits one item to be the right-hand side (RHS) and the remaining items as the left-hand side (LHS).

### 3.2 Choosing an appropriate Minimum Confidence Threshold
The min confidence threshold is determined based on the confidence distribution:
- Calculate the confidence of all association rules from the dataset.
- Identify the confidence value at the 75th percentile.
- Set the minimum confidence threshold at this level or higher to retain the most reliable rules.
![image](https://github.com/user-attachments/assets/0331891b-5315-4b92-819c-2d7d351200ec)
![image](https://github.com/user-attachments/assets/471b594b-1c1a-4ca7-b4a0-6ea7b91f84a3)
The author selected the Min Confidence threshold of 0.50 based on the Elbow method, as the number of strong association rules drops significantly and stabilizes beyond this point.

## Step 4: Evaluating Association Rules and identifying Strong Rules using the LIFT metric
There are 244 association rules with LIFT > 1, which indicates that these rules reflect a positive correlation between the items in each rule.  
Filter out rules with LIFT > 1 and confidence >= min confidence, we have 11 strong association rules.  
![image](https://github.com/user-attachments/assets/9eb2c8fb-c52f-4cc6-b706-127e9fa69a20)
