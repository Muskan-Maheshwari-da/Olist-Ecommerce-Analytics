# Olist E-Commerce Performance & Executive Analytics

[![Power BI Dashboard](https://img.shields.io/badge/📥_Power_BI_Dashboard-.PBIX-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)](./olist.pbix)
[![Python Notebook](https://img.shields.io/badge/🐍_Python_ETL-.IPYNB-3776AB?style=for-the-badge&logo=python&logoColor=white)](./Olist.ipynb)

End-to-end analysis of a Brazilian online marketplace using **Python, SQL and Power BI**, to understand revenue, delivery performance and customer satisfaction.

![Olist Executive Dashboard](./Olist%20dashboard%20image.png)

---
## Business Problem
Management wants to know how much revenue the marketplace generates, how often deliveries are late, how late deliveries affect customer reviews, and which regions need attention.

## Tools & Skills
- **Python:** pandas, numpy, matplotlib, seaborn (cleaning, feature engineering, validation)
- **SQL (SQLite):** views, aggregation, CASE logic, monthly and state-wise analysis
- **Power BI:** KPI cards, map, donut and bar charts, date slicer

## Dataset
[Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle): 9 related tables, 99,441 orders, 2016 to 2018. Currency is Brazilian real (BRL).

## Approach
1. **Load and check:** loaded 9 CSV files and checked shape and missing values for every table (for example, 7,873 missing values in orders, mostly undelivered orders with no delivery date).
2. **Clean and engineer features:** converted date columns, created `delivery_delay_days` (actual vs estimated delivery) and an `is_late` flag, and added English category names (missing ones set to "Unknown").
3. **Aggregate before joining:** reviews and payments were combined to one row per order, so orders and revenue are not duplicated.
4. **Validate:** built an order-level master table of 99,441 rows and used assertions to confirm one row per order.
5. **SQL analysis:** loaded the data into SQLite, created a view (`vw_order_summary`) and wrote queries for monthly revenue, late % by state, and review score by delay bucket.
6. **Dashboard:** exported clean CSV files and built the Power BI dashboard.

## Dashboard KPIs
| KPI | Value |
|---|---|
| Total Revenue (total payment value) | 16.01M |
| Total Orders | 99K |
| Late Delivery Rate | 6.57% |
| Average Review Score | 4.09 |

Visuals: revenue by year, orders by status, revenue by customer state (map), review score by late delivery, and orders by payment installments.

## Key Insights
1. **Orders are mostly fulfilled:** about 97% of orders (96K) are delivered.
2. **Late delivery is a minority problem:** 6.57% of orders are late.
3. **Late orders get much lower reviews:** average review is about 4.2 for on-time orders and about 2.3 for late orders (approx., from the dashboard). The correlation between delay days and review score is about -0.27.
4. **Some states struggle most:** AL, MA and PI have the highest late-delivery rates (roughly 14% to 24%, depending on how lateness is defined).
5. **Revenue is growing:** 2018 revenue is already above 2017 even though 2018 data is incomplete.
6. **Payments:** about 48.5% of orders are paid in one installment and about 12.4% in two; the rest are spread across 3 to 10.

## Recommendations
1. Add buffer days to delivery promises and review carriers in the highest-late states (AL, MA, PI).
2. Contact customers proactively when an order passes its promised date.
3. Track on-time % and review score by state every month.
4. Encourage sellers to stock closer to high-demand regions to cut shipping time.
5. Investigate low-review categories, since lateness is not the only cause of bad reviews.

## How to Run
```bash
pip install pandas numpy matplotlib seaborn kagglehub jupyter
jupyter notebook Olist.ipynb
```
The notebook downloads the dataset from Kaggle automatically (`kagglehub`). It creates `olist_analytics.db` and the CSV files used in Power BI.

## Repository Structure
```
├── Olist.ipynb                      # Python + SQL analysis
├── olist_powerbi_master.csv         # order-level data for Power BI
├── state_performance_powerbi.csv    # state-wise late % data
├── images/                          # dashboard screenshot
└── README.md
```

## Limitations
- Data is from 2016 to 2018 and from one Brazilian marketplace, so exact numbers do not apply to a company today, but the method does.
- Reviews exist only for customers who responded.
- Late deliveries and low reviews move together, but this analysis does not prove lateness alone causes bad reviews.
