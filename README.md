# End-to-End Loan Default Prediction System 

![Dashboard](Assets/title.png)

## Overview 

Lending Club is a peer-to-peer Lending company based in the US. They match people looking to invest money with people looking to borrow money. When investors invest their money through Lending Club, this money is passed onto borrowers, and when borrowers pay their loans back, the capital plus the interest passes on back to the investors. It is a win for everybody as they can get typically lower loan rates and higher investor returns. 

## Use Cases 

## Folder Structure 

## Project Workflow 

## Dataset 

The project based on a scaled down [Dataset](https://www.kaggle.com/datasets/janiobachmann/lending-club-first-dataset). The [Lending Club original dataset](https://www.kaggle.com/datasets/adarshsng/lending-club-loan-data-csv/data) contains complete loan data for all loans issued through the 2007-2015, including the current loan status (Current, Late, Fully Paid, etc.) and latest payment information. Features (aka variables) include credit scores, number of finance inquiries, address including zip codes and state, and collections among others. Collections indicates whether the customer has missed one or more payments and the team is trying to recover their money. The file is a matrix of about 890 thousand observations and 75 variables. 

| Feature Group                     | Columns                                                                                                                                         | Data Type                    | Description                                                                                 |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------- |
| **Loan Identification**           | id, member_id, url, policy_code, initial_list_status                                                                                            | Integer, String, Categorical | Unique identifiers and platform information about the loan listing                          |
| **Loan Information**              | loan_amnt, funded_amnt, funded_amnt_inv, term, int_rate, installment, grade, sub_grade, loan_status                                             | Numeric, Categorical         | Core loan details including amount, term, interest rate, installment and loan status        |
| **Borrower Personal Information** | emp_title, emp_length, home_ownership, zip_code, addr_state                                                                                     | String, Categorical          | Borrower's employment details and residential information                                   |
| **Income & Verification**         | annual_inc, verification_status, annual_inc_joint, verification_status_joint, application_type                                                  | Numeric, Categorical         | Borrower income details and whether income was verified                                     |
| **Loan Purpose Information**      | purpose, title, desc                                                                                                                            | Categorical, String, Text    | Borrower-stated purpose and description of the loan                                         |
| **Credit Profile**                | dti, dti_joint, earliest_cr_line, total_acc, open_acc, pub_rec, revol_bal, revol_util                                                           | Numeric, Integer, Date       | Borrower credit history including debt-to-income ratio, credit lines and revolving balances |
| **Credit History & Delinquency**  | delinq_2yrs, mths_since_last_delinq, mths_since_last_record, acc_now_delinq, collections_12_mths_ex_med, mths_since_last_major_derog            | Integer, Numeric             | Indicators of borrower delinquency and derogatory credit events                             |
| **Credit Inquiry Activity**       | inq_last_6mths, inq_last_12m, inq_fi                                                                                                            | Integer                      | Number of credit inquiries within recent time periods                                       |
| **Revolving Credit Activity**     | open_rv_12m, open_rv_24m, max_bal_bc, total_rev_hi_lim                                                                                          | Integer, Numeric             | Revolving credit account activity and credit limits                                         |
| **Installment Account Activity**  | open_il_6m, open_il_12m, open_il_24m, total_bal_il, il_util, mths_since_rcnt_il                                                                 | Integer, Numeric             | Installment loan activity and installment credit utilization                                |
| **Account Balance Metrics**       | tot_cur_bal, tot_coll_amt, all_util, total_cu_tl                                                                                                | Numeric, Integer             | Overall credit balances, collections and financial trade counts                             |
| **Payment & Loan Performance**    | out_prncp, out_prncp_inv, total_pymnt, total_pymnt_inv, total_rec_prncp, total_rec_int, total_rec_late_fee, recoveries, collection_recovery_fee | Numeric                      | Payment performance and recovery amounts                                                    |
| **Date Features**                 | issue_d, last_pymnt_d, next_pymnt_d, last_credit_pull_d                                                                                         | Date                         | Important loan timeline events                                                              |
| **Recent Account Activity**       | open_acc_6m                                                                                                                                     | Integer                      | Number of credit accounts opened recently                                                   | 

![Dashboard](Assets/null_value_count.png) 

## Data Cleaning 

```bash
{'Does not meet the credit policy. Status:Fully Paid': 'Fully Paid',
 'Does not meet the credit policy. Status:Charged Off': 'Charged Off',
 'Default': 'Charged Off'}
```

Keep features: 

`loan_amnt` `term` `int_rate` `installment` `grade` `sub_grade` `annual_inc` `verification_status` 

`dti` `earliest_cr_line` `open_acc` `total_acc` `revol_bal` `revol_util` `all_util` `delinq_2yrs` 

`acc_now_delinq` `mths_since_last_delinq` `pub_rec` `collections_12_mths_ex_med` `inq_last_6mths` 

`inq_last_12m` `tot_cur_bal` `total_rev_hi_lim` `home_ownership` `addr_state` `emp_length` `purpose` 

Target: `loan_status` 

## Feature Engineering 

SHAP feature importance 

## Exploratory Data Analysis 

## Note 

`grade` will not be used for training. But, input will be taked in deployment. Upon selection, `sub_grade` will be filetered (for example, if A is selected for grade, users can choose from A1 to A5 instead of A1 to G5). 

`region` field will be created during feature engineering section. In deployment, `addr_state` will be filtered according to `region` selection. 

| Region          | Count  |
| --------------- | ------ |
| West            | **11** |
| South           | 14     |
| North           | 8      |
| Central         | 12     |
| East            | 4      |
| Out of Mainland | 2      | 

```bash
region_map = {

# WEST
'CA':'West','OR':'West','WA':'West','NV':'West','AZ':'West','UT':'West','ID':'West',

# SOUTH
'TX':'South','FL':'South','GA':'South','NC':'South','SC':'South','VA':'South',
'TN':'South','AL':'South','MS':'South','LA':'South','AR':'South','OK':'South',
'KY':'South','WV':'South',

# NORTH
'NY':'North','NJ':'North','MA':'North','CT':'North','RI':'North',
'VT':'North','NH':'North','ME':'North',

# CENTRAL
'IL':'Central','MO':'Central','MN':'Central','WI':'Central','MI':'Central',
'IA':'Central','KS':'Central','NE':'Central','SD':'Central','ND':'Central',
'IN':'Central','OH':'Central',

# EAST
'PA':'East','MD':'East','DE':'East','DC':'East',

# OUT OF MAINLAND
'AK':'Out_Mainland','HI':'Out_Mainland'
}
```

```bash
df['region'] = df['addr_state'].map(region_map).fillna('Unknown')
```

Create `revol_util_risk` field from `revol_util` column: 

| Utilization | Risk     |
| ----------- | -------- |
| 0–30%       | healthy  |
| 30–60%      | moderate |
| 60% +       | high     | 

Create `dti_risk_level` from `dti` column: 

| DTI   | Interpretation  |
| ----- | --------------- |
| 0–20  | low debt burden |
| 20–35 | manageable      |
| 35–50 | high            |
| 50 +  | extremely risky | 

| grade | sub_grade | Meaning     |
| ----- | --------- | ----------- |
| A     | A1–A5     | lowest risk |
| B     | B1–B5     | low risk    |
| C     | C1–C5     | medium risk |
| D–G   | D1–G5     | higher risk | 

## Deployment Guideline 

| Feature Category                 | Important Features                                       | Why Important                                                            |
| -------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------------------ |
| **Loan Characteristics**         | loan_amnt, term, int_rate, installment, grade, sub_grade | Reflect risk level assigned by the lending platform and repayment burden |
| **Income & Financial Stability** | annual_inc, verification_status, dti                     | Measure borrower’s ability to repay debt                                 |
| **Credit History**               | earliest_cr_line, total_acc, open_acc                    | Longer and broader credit history usually indicates lower risk           |
| **Credit Utilization**           | revol_bal, revol_util, all_util                          | High utilization often signals financial stress                          |
| **Delinquency History**          | delinq_2yrs, acc_now_delinq, mths_since_last_delinq      | Past delinquencies strongly correlate with default risk                  |
| **Public Records / Collections** | pub_rec, collections_12_mths_ex_med                      | Legal or collection issues indicate higher risk                          |
| **Inquiry Behavior**             | inq_last_6mths, inq_last_12m, inq_fi                     | Frequent credit inquiries may indicate financial distress                |
| **Installment Account Activity** | open_il_12m, open_il_24m, total_bal_il                   | Indicates recent borrowing behavior                                      |
| **Revolving Credit Behavior**    | open_rv_12m, open_rv_24m, max_bal_bc                     | Shows revolving credit activity and risk                                 |
| **Total Credit Exposure**        | tot_cur_bal, total_rev_hi_lim                            | Overall borrower debt exposure                                           |
| **Borrower Profile**             | home_ownership, emp_length                               | Indicators of financial stability                                        | 

Sub Grade will be auto filtered upon selecting Grade. Once Sub Grade is selected, Interest Rate and risk_tier will be auto filled. If user manually change interest rate, Grade, Sub Grade and risk_tier will be dynamically updated. 

| Grade | Sub Grade | Suggested Interest Rate |
| ----- | --------- | ----------------------- |
| A     | A1        | 6.0%                    |
| A     | A2        | 6.5%                    |
| A     | A3        | 7.0%                    |
| A     | A4        | 7.5%                    |
| A     | A5        | 8.0%                    |
| B     | B1        | 8.5%                    |
| B     | B2        | 9.0%                    |
| B     | B3        | 9.5%                    |
| B     | B4        | 10.0%                   |
| B     | B5        | 10.5%                   |
| C     | C1        | 11.0%                   |
| C     | C2        | 12.0%                   |
| C     | C3        | 13.0%                   |
| C     | C4        | 14.0%                   |
| C     | C5        | 15.0%                   |
| D     | D1        | 16.0%                   |
| D     | D2        | 17.0%                   |
| D     | D3        | 18.0%                   |
| D     | D4        | 19.0%                   |
| D     | D5        | 20.0%                   |
| E     | E1        | 21.0%                   |
| E     | E2        | 22.0%                   |
| E     | E3        | 23.0%                   |
| E     | E4        | 24.0%                   |
| E     | E5        | 25.0%                   |
| F     | F1        | 26.0%                   |
| F     | F2        | 27.0%                   |
| F     | F3        | 28.0%                   |
| F     | F4        | 29.0%                   |
| F     | F5        | 30.0%                   |
| G     | G1        | 31.0%                   |
| G     | G2        | 32.0%                   |
| G     | G3        | 33.0%                   |
| G     | G4        | 34.0%                   |
| G     | G5        | 35.0%                   | 


| risk_tier       | grade | sub_grade |
| --------------- | ----- | --------- |
| Low Risk        | A     | A1–A5     |
| Low-Medium Risk | B     | B1–B5     |
| Medium Risk     | C     | C1–C5     |
| High Risk       | D–G   | D1–G5     | 

## Deployment 

<p align="center">
  <img src="Assets/image 1.png" width="34.5%" />
  <img src="Assets/image 3.png" width="36%" />
  <img src="Assets/image 4.png" width="27%" />
</p>



















