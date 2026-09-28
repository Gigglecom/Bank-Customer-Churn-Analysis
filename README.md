# Bank Customer Churn Analysis Using SQL Server

## Project Overview

This project analyzes customer churn in a banking dataset using Microsoft SQL Server and SQL Server Management Studio (SSMS).

The goal was to identify customer characteristics associated with churn, segment customers by risk level, and generate business insights that could support customer-retention decisions.

The dataset contains 10,000 customer records with information such as age, geography, credit score, account balance, number of products, activity status, estimated salary, and churn status.

## Tools Used

- Microsoft SQL Server
- SQL Server Management Studio (SSMS)
- SQL
- GitHub

## SQL Skills Demonstrated

- SELECT statements
- WHERE and CASE statements
- GROUP BY and HAVING
- Aggregate functions
- Common Table Expressions (CTEs)
- Window functions
- RANK()
- NTILE()
- Views
- Stored Procedures
- Customer segmentation
- Conditional aggregation
- Data-quality checks

## Dataset Fields

Key columns used in the analysis include:

- CustomerId
- CreditScore
- Geography
- Gender
- Age
- Tenure
- Balance
- NumOfProducts
- HasCrCard
- IsActiveMember
- EstimatedSalary
- Exited

`Exited = 1` represents a customer who churned, while `Exited = 0` represents a retained customer.

## Data Quality Checks

Before performing the analysis, the dataset was checked for:

- Duplicate Customer IDs
- Missing values
- Invalid categorical values
- Customer ID uniqueness
- Appropriate data types

No duplicate Customer IDs were found, allowing `CustomerId` to be used as the primary key.

## Analysis Performed

The project examined:

- Overall customer churn rate
- Churn by geography
- Churn by gender
- Churn by active-member status
- Churn by number of products
- Churn by age group
- Churn by credit-score category
- Churn by account-balance category
- Customer balance quartiles using NTILE()
- Geography ranking using RANK()
- Rule-based customer risk segmentation

## Customer Risk Segmentation

A rule-based churn risk score was created using customer characteristics including:

- Activity status
- Age
- Account balance
- Number of products
- Credit score

Customers were classified into:

- High Risk
- Medium Risk
- Low Risk

The resulting churn rates were:

| Risk Category | Customers | Churned | Churn Rate |
|---|---:|---:|---:|
| High Risk | 1,471 | 739 | 50.24% |
| Medium Risk | 4,702 | 1,098 | 23.35% |
| Low Risk | 3,827 | 200 | 5.23% |

This risk score is a rule-based analytical segmentation and not a machine-learning prediction model.

## Key Insights

Inactive customers showed substantially higher churn than active customers.

Customers with three or four banking products experienced very high churn rates, while customers with two products had the lowest churn rate.

Churned customers were older on average than retained customers.

The risk-segmentation framework clearly separated customers into groups with different observed churn rates.

High-risk customers who had not yet exited were identified as a potential priority group for customer-retention efforts.

## Database Objects Created

The project also included reusable SQL Server objects such as:

### Views

- `vw_CustomerRiskSegmentation`
- `vw_ChurnKPIs`
- `vw_RiskCategorySummary`

### Stored Procedures

- `sp_GetCustomersByRisk`
- `sp_GetChurnByGeography`

## Example Business Question

One of the key business queries answers:

> Which high-risk customers are still active with the bank and could be prioritized for retention?

```sql
SELECT
    CustomerId,
    Surname,
    Geography,
    Age,
    CreditScore,
    Balance,
    NumOfProducts,
    IsActiveMember,
    RiskScore,
    RiskCategory
FROM vw_CustomerRiskSegmentation
WHERE RiskCategory = 'High Risk'
  AND Exited = 0
ORDER BY RiskScore DESC, Balance DESC;
