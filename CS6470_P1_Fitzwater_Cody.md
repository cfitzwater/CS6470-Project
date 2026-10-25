# CS6470_P1: Applied Project Proposal

### 1. The decision
A customer service representative for a cell phone and internet provider reviews customer accounts every Monday at the beginning of their shift to determine which subscription accounts would benefit from promotional offerings to reduce the risk of churn. The customer service representative has the capacity to reach out to 25 high risk accounts each week. The model will rank monthly subscribers by risk for churn and will identify the 25 highest risk customers likely to cancel their contract, allowing the customer service rep to reach out and proactively retain the customer before its too late.
 
### 2. The data
- **Source:** Our dataset is the IBM Telco Customer Churn public benchmark dataset and is hosted on Kaggle: `https://www.kaggle.com/datasets/blastchar/telco-customer-churn`.
- **Permission:** The dataset is publicly available with no proprietary corporate agreements, NDAs, or institutional review board permissions required.
- **Freshness:** The data is historical account information that is captured at the end of each month.
- **Row/Column Sketch:**
  - **Dataset Dimensions:** The dataset has 7,043 customer rows and 21 columns.
  - **One row represents:** Each row represents a single customer account.
  - **Outcome Column:** We have an outcome column `Churn` which tells the model which customers canceled their subscriptions and which didn't (Yes or No).
    - **Feature Columns:** Some of our featured columns most likely to help our model predict customer churn inlude:
    - `tenure`: This is how long (months) the customer has been on service.
    - `MonthlyCharges`: The current month to month cost of the customers service.
    - `TotalCharges`: Total cost to the customer over the lifetime of their contract.
    - `Contract`: Terms of their contract whether is is month to month or annually.
    - `TechSupport`: Does the customer have premium tech support or not (Yes/No)

### 3. Success metric
To determine success I will be using Precision@K where K=25, due to the customer service rep only being able to reach out to 25 customers per week. This will allow us to hone in on the 25 highest risk customers each week and improve the customer service reps efficiency. I am avoiding basic accuracy because only about 26.5% of customers actually churn. A simple model could guess "No Churn" every single time and be "accurate" 73.5% of the time, while completely failing to catch a single customer walking out the door. In P2, we will compare our model against both a dummy classifier that guessed the majority (no for everyone) and a simple logistic regression model. This will allow us to determine if we will need to look at a more complex approach.

### 4. Leakage plan
To prevent data leakage I plan to drop the customerID column because it provides no predictive value and is only used as a tracking number. I have reviewed the other 20 columns and verified they have information that will be beneficial to our predictive model, and there is no data in any of the columns that is entered post cancellation. To avoid leaking validation data into the training process, the data splitting will occur before missing values are filled in and will use a scikit-learn pipeline.

### 5. Ethics and bias
Bias could be introduced into the model through data in 2 columns that serve as proxies for Age. The two columns at risk for introducing systematic bias are `PaymentMethod`, and `PhoneService`, as these are all possible indicators for a protected attribute `SeniorCitizen`. In P2 I will use the A4 slice technique to divide the data into older customers and younger customers to evaluate error rates for each slice to make sure our model isn't introducing bias into the system.
I also plan to run the A4 slice technique for `Dependents` and `Partner` as well to ensure there is no bias towards larger households with families. Otherwise, families could find themselves helping to subsidize premium discounts for households with no dependents.

### 6. Toolkit
I will be using standard Python libraries: **pandas** for tabular ingestion and data type conversions, and **scikit-learn** for pipeline building, cross-validation, and baseline modeling. No deep learning frameworks or specialized external libraries are required.

### 7. Risks and fallback
The most likely cause of failure for this plan would be the `TotalCharges` column where there are 11 rows that contain invisible whotespace strings. This could cause the model to crash so I will need to make sure this column is setup as numeric floats when the data is first loaded. If our dataset doesnt provide the expected results then the Plan B Dataset would be **Bank Marketing Dataset (UCI Machine Learning Repository)** because of its similarities to our current dataset.
