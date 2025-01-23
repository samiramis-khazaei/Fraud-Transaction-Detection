The dataset contains 29 columns and 78,363 observations collected from 2016-08-12 to the end of 2016. The target column, ‘isFraud’, indicates whether a transaction is fraudulent:
- True: Fraudulent transactions.
- False: Non-fraudulent transactions.
  
The dataset is highly unbalanced, with 98.42% non-fraud and 1.58% fraud transactions, and The Shape of the datase is  (78,363, 29) and the types of data are: 
- Object (strings): 21 columns (e.g., accountNumber, merchantName).
- Float64 (numerical): 4 columns (e.g., transactionAmount, currentBalance).
- Boolean: 3 columns (cardPresent, expirationDateKeyInMatch, isFraud).
### **Data Cleaning  and Transformation :***
- Some columns contain blank values instead of NaN, requiring cleaning. Columns with all blank values (e.g., ['echoBuffer', 'merchantCity', 'merchantState', 'merchantZip', 'posOnPremises', 'recurringAuthInd']) do not provide any useful data and will be dropped.
- Fill the missing values of columns with fewer than 10% blank values with 'Other'
- The columns accountNumber and customerId contain identical data, so accountNumber will be dropped.
- A new column ‘cvvMatch’ will be created to indicate whether the cardCVV differs from the enteredCVV, after this, the columns cardCVV and enteredCVV will be dropped.
- ["merchantName", "merchantCategoryCode", "customerId", "cardLast4Digits"] have a high number of categories. Also, customerId and cardLast4Digits do not have predictivity power. So I drop these two columns, and for the other columns, I only choose the top 5 categories and replace the other categories with Other. 

## Exploratory Data Analysis:
- **Question one: Does Fraud Occur Only Among Customers with a High Number of Transactions?**
To statistically assess the relationship between total transaction volume and fraud, the Spearman correlation coefficient was calculated, and a correlation of 0.0183 is almost negligible, confirming that there is no meaningful relationship between the number of transactions a customer makes and the likelihood of fraud.

- **Question Two: Does a merchant have a higher potential to commit fraud?"**
The correlation coefficient of 0.0537 indicates a very weak positive relationship between the total number of transactions per merchant and fraud. In other words: Merchants with higher transaction volumes do not necessarily have a higher likelihood of fraud.

- **Exploring Time-Related Features**

![image](https://github.com/user-attachments/assets/83c86da1-2ca8-4ee4-bfc3-f7de2cec96c0)

Hourly Insights: 
- All Transactions: The highest number of transactions occurs around 4 PM (16:00).
- Fraudulent Transactions: Peak fraud transactions occur around 1 PM (13:00), and a round 3 PM (15:00): 1- The sum of transaction amounts is highest for fraud transactions.2-The mean transaction amount for fraudulent transactions is lowest compared to other times.

![image](https://github.com/user-attachments/assets/392f08c9-4a1a-4d66-b4d8-2014f9003dab)

Daily Insights: 
- All Transactions: The number of transactions remains consistent across all days of the month.
- Fraudulent Transactions: Peaks occur on the 17th day and between the 27th to 28th days of the month.
+ Regarding transaction amounts: The sum of fraudulent transaction amounts is highest on the 3rd day and the lowest on the 17th day.

+ ![image](https://github.com/user-attachments/assets/f9c96f0e-5e7b-4083-ab50-307668a566bf)

Monthly Insights
All Transactions:
- **Transaction count:**
  - Increases throughout the year.
  - Highest: December.
  - Lowest: February.
- **Transaction amount (sum):**
  - Highest: October.
  - Lowest: February.
- **Transaction amount (mean):**
  - Highest: January.
  - Lowest: December.
  - The overall trend has a downward slope.

Fraudulent Transactions:
- **Transaction count fluctuates:**
  - Highest: June.
  - Lowest: November.
- **Transaction amount (sum):**
  - Highest: June.
  - Lowest: November.
- **Transaction amount (mean):**
  - Highest: June.
  - Lowest: November.

**Risk Analysis Based on Crosstab**

By looking at the crosstab of the columns `['acqCountry', 'merchantCountryCode', 'posEntryMode', 'posConditionCode', 'merchantCategoryCode', 'transactionType', 'cardPresent', 'expirationDateKeyInMatch']`, we can see these categories have higher risk:

- **Rideshare** followed by **online_retail** have a higher frequency of fraud.
- **Transactions where the card is not present** have a higher risk.
- **Transactions where the expiration date key is not in March** are more risky.
- **Transactions of the "Other" type** pose a higher risk.
- **Post entry mode is "Other"** has a higher risk.
- **Post condition code is "Other"** indicates higher risk.
- **Acquiring country is "Other"** shows a higher frequency of fraud.
- **Merchant country code is "Other"** shows higher fraud risk.

**Numerical Columns**

![image](https://github.com/user-attachments/assets/5252341f-8a5c-47d9-b963-c5f00d76934d)

- **Credit Limit (`creditLimit`):**
  - Fraudulent and non-fraudulent transactions overlap significantly.
  - Fraud cases seem slightly skewed toward lower credit limits, but the difference is subtle.

- **Available Money (`availableMoney`):**
  - Both distributions are similar.
  - Fraudulent transactions are slightly more concentrated in the lower range of available money.

- **Transaction Amount (`transactionAmount`):**
  - Fraudulent transactions are concentrated at lower transaction amounts (peaking at smaller values like below 250).
  - Non-fraudulent transactions have a wider spread, indicating a more diverse transaction amount distribution.

- **Current Balance (`currentBalance`):**
  - Fraudulent transactions have a skew toward smaller balances.
  - Non-fraudulent transactions are distributed more evenly across the range.

- **Card Present (`cardPresent`):**
  - Binary feature (0 or 1).
  - Fraudulent and non-fraudulent distributions overlap almost perfectly, indicating this feature might not differentiate fraud well.

- **Expiration Date Match (`expirationDateKeyInMatch`):**
  - Binary feature.
  - Fraudulent and non-fraudulent distributions are nearly identical, showing minimal usefulness for fraud detection.

- **CVV Match (`cvvMatch`):**
  - Binary feature.
  - Similar to the expiration date match, this feature doesn't seem to add much distinction between fraud and non-fraud.

**Correlation Matrix**

![image](https://github.com/user-attachments/assets/9914e742-c254-4474-9d73-e9e3d8b5add9)

The heatmap shows only availabeMoney and creditLimit columns are highly correlated and the correlation of features and the target almost zero.
