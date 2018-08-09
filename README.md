# homeCreditDefaultRisk
Data Analytics Applications
The goal of this project is to predict who is going to default on their home loans. 

Below are the steps followed in this project


Data Overview
Preprocessing
Feature Engineering 
Modelling 
Results

Application table (train) 
80 numerical 42 categorical.
Row count: 307.5K
9.1 million missing cells out of 37.5 million cells. (24.3% Missing)
Includes ;
Demographics (Income, gender, family info, education, age etc.)
Application document flags (21 variables ,decoded)
Valuable Possessions (Car, Age of car, House etc.)
Contact information flags (Email, Cell Phone etc)
Detailed living area details (Wall materials, windows , elevators etc.)
Some decoded information (3 variables)

Previous Applications
38 variables, 1.6 million rows

Details of previous credit application 
Type, purpose , interest, down payment , payment method , insurance , 
New/old client , who accompanied the client , time and date details etc.


Credit Card Balance Table
23 variables, 3.8million rows

Detailed Credit Card information 
Limit, Balance, Number of usage, cash withdrawals , interest rate 
Payment details, days past due.


Installment Payments Table
8 variables, 13.6million rows

Payment history on previous Home Credit loans. 
Payment amounts and dates.
Payment method.
Type of loan.
Due dates.



POS Cash Balance Table
8 variables, 10 million rows
Provides historical information relative to the application date.
Overdue information for each month.
Active or Closed.
Remaining instalment counts.



Bureau
17 variables, 1.7 million rows
Previous application information from other institutions.
Date and time info relative to Home Credit loan app.
Maximum amount overdue.
Total debt, limit , overdue, annuity.
Currency.
Time of report. 



Bureau Balance
3 variables, 27.2 million rows
Month of balance relative to the application date. 
Date and the status of the loan. 




Separated 78 numerical variable and 42 categorical variables.
21 numerical were eliminated (+30% missing)
12 numerical eliminated NZV. (45 remained)
Experimented with several imputation methods (KNNinpute, Mean , PMM, rf, randomSample, norm.nob )
Fixed data some errors. 
replace(trainNum$DAYS_EMPLOYED,trainNum$DAYS_EMPLOYED == 365243,NA)



Feature Engineering

From Bureau data set

Created a percentage score that indicates how close the individuals is to maximize their limit (most recent month). 

AMT_BALANCE/AMT_CREDIT_LIMIT_ACTUAL
aveBalLimCurPropCRED

 Created a  percentage score  for meeting minimum payment requirement.

ifelse(AMT_PAYMENT_CURRENT>=AMT_INST_MIN_REGULARITY,1,0)
AvgPercMinPaymMadeCRED




From installment data set
Calculated a score for each account. 

Total Amount paid / total amount (since the loan became active). 

Took the average off all the account scores for each person

aveLoanPaidScoreINSTALL



From POS cash balance data set

Number installments paid / sum of total installment count.

percentPaidPOS



From Previous application table

Sum of loan application refusal count across all previous applications. 

refusalCountScorePREV




Additionally following variables created.
Since different currencies are involved , we divided the following variable by personal income.

creditSUM_Income=AMT_CREDIT_SUM/AMT_INCOME_TOTAL
creditSUMlimit_Income=AMT_CREDIT_SUM_LIMIT/AMT_INCOME_TOTAL
creditSUMDebt_Income=AMT_CREDIT_SUM_DEBT/AMT_INCOME_TOTAL
creditSUMOverdue_Income=AMT_CREDIT_SUM_OVERDUE/AMT_INCOME_TOTAL



Modelling


First we started with the Application Train Data and got maximum of 62% AUC
We used Logistic, Random Forest, XGBoost


Model evaluation

Precision vs Recall and F1 
Recall = Specificity(-) and Sensitivity(+)
Amongst all criminals how many did you catch?
Precision=
Amongst all the people you caught how many of them were actual criminals ?
Similar to the Type1 and Type2 error notion.
F1 ratio takes both in to account.
Also used Kappa and AUC to evaluate the models.


Model Results


Logistic (Imbalanced)
AUC   0.6985
KAPPA 0.004804478
F1    0.006085193
Logistic (Down Sampled)
AUC   0.6983
KAPPA 0.1119984
F1    0.2287099
Random Forest
AUC   0.7082
KAPPA 0.1188
F1    0.2338514
XGboost
AUC   0.753618
KAPPA 0.0307 













