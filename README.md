# Telco Customer Churn: Who Should the Retention Team Call First?

A telecom company is losing about 1 in 4 customers. This project predicts which customers are likely to leave. It then goes beyond model accuracy to answer the business question: **which customers should the retention team contact first, and how much revenue would that save?**

![Revenue saved](figures/revenue_saved.png)

## Dataset

This project uses the [Telco Customer Churn dataset](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) from Kaggle (IBM sample data). It has 7,043 customers and 21 columns covering contract, services, billing and whether the customer left.

## Key results

- **The model ranks customers well.** The riskiest 10% of customers churn at almost 3x the average rate, and the riskiest 30% include two-thirds of everyone who leaves (ROC-AUC 0.84).
- **One segment carries over a third of the risk.** Month-to-month customers in their first year on fibre-optic internet account for **37% of all revenue at risk**, with 916 customers and a 68% chance of leaving.
- **Targeting pays, and contacting everyone doesn't.** Contacting the riskiest 25% saves about **$155K a year** in net revenue. Contacting the same number at random *loses* $52K, and contacting everyone loses $203K.

| Priority | Segment | Customers | Chance of leaving | Share of revenue at risk |
|---|---|---|---|---|
| 1 | Month-to-month, 0–12 months, fibre | 916 | 68% | 37% |
| 2 | Month-to-month, 13–24 months, fibre | 425 | 58% | 16% |
| 3 | Month-to-month, 25–48 months, fibre | 521 | 45% | 16% |
| 4 | Month-to-month, 0–12 months, DSL | 690 | 39% | 9% |

![Priority segments](figures/priority_segments.png)

## What I did

1. **Cleaned the data:** fixed a text column that should have been numbers and filled 11 blank values for brand-new customers.
2. **Explored what drives churn:** contract type, how long someone has been a customer, internet type and payment method.
3. **Built and compared two models:** logistic regression and gradient boosting. Both scored about the same, so I used logistic regression because it's easier to explain.
4. **Turned predictions into money:** revenue at risk = chance of leaving × yearly bill.
5. **Ranked segments** that the retention team can act on directly.
6. **Estimated revenue saved** from a retention campaign, compared with random targeting, and tested how the result changes under different assumptions.

![What drives churn](figures/churn_drivers.png)

## Assumptions for the revenue estimate

- A retention offer costs **$100** per customer contacted.
- The offer keeps **30%** of customers who would otherwise leave.
- A saved customer is worth **12 months** of their bill.

The notebook shows how the savings change if the offer works better or worse (15–45%) or costs more or less ($50–$150).

## Recommendations

1. Contact new month-to-month fibre customers first.
2. Offer them an upgrade to a 1- or 2-year contract. Those customers churn at 11% and 3%, compared with 43% for month-to-month.
3. Encourage automatic payment instead of electronic check (45% churn).
4. Contact only the riskiest ~25% of customers. Beyond that, the offers mostly go to people who would have stayed anyway.
5. Test the offer on a small group first to confirm the 30% success rate.

## Tools

Python, pandas, scikit-learn, matplotlib, Jupyter

## How to run

1. Download the dataset from Kaggle and put the CSV file in the `data/` folder.
2. Install the libraries: `pip install -r requirements.txt`
3. Open `notebooks/churn_analysis.ipynb` and run all cells.
