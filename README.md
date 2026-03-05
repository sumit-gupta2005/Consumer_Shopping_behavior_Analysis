# Consumer_Shopping_behavior_Analysis

### End-to-End Data Analytics Project (Python • SQL • Power BI)

## Project Overview

This project analyzes **consumer purchasing behavior** using an end-to-end data analytics pipeline. The objective is to transform raw retail transaction data into **meaningful business insights** that help organizations understand customer behavior, product performance, and revenue patterns.

The project demonstrates the complete workflow of a **data analyst**, including:

* Data Cleaning and Feature Engineering (Python)
* Business Analysis (SQL)
* Data Visualization (Power BI)


# Project Architecture

```
Raw Dataset
     ↓
Data Cleaning (Python - Pandas)
     ↓
Feature Engineering
     ↓
Data Storage (MySQL)
     ↓
Business Analysis (SQL Queries)
     ↓
Data Visualization (Power BI Dashboard)
     ↓
Business Insights
```

---

# Dataset Information

The dataset contains **3,900 customer purchase records** with **18 attributes** describing customer demographics, product details, and purchasing behavior.

# Data Cleaning & Feature Engineering (Python)

Data preprocessing was performed using **Pandas and NumPy**.

### Import Libraries

```python
import pandas as pd
import numpy as np
```

### Load Dataset

```python
df = pd.read_csv('customer_shopping_behavior.csv')
```

### Standardize Column Names

```python
df.columns = df.columns.str.lower()
df.columns = df.columns.str.replace(' ', '_')
df = df.rename(columns={'purchase_amount_(usd)': 'purchase_amount'})
```

### Handle Missing Values

Missing values in `review_rating` were replaced using the **median rating of each category**.

```python
df['review_rating'] = df.groupby('category')['review_rating'].transform(
    lambda x: x.fillna(x.median())
)
```

### Create Age Groups

```python
labels = ['young','adult','middle_aged','senior']
df['age_group'] = pd.qcut(df['age'], q=4, labels=labels)
```

### Convert Purchase Frequency to Days

```python
frequency_mapping = {
'Fortnightly':14,
'Weekly':7,
'Monthly':30,
'Quarterly':90,
'Bi-Weekly':14,
'Annually':365,
'Every 3 Months':90
}

df['frequency_purchase_days'] = df['frequency_of_purchases'].map(frequency_mapping)
```

---

# SQL Business Analysis

After preprocessing, the dataset was imported into **MySQL** for analysis.

### Revenue by Gender

```sql
SELECT gender, SUM(purchase_amount) AS total_revenue
FROM consumer
GROUP BY gender;
```

### Top 5 Highest Rated Products

```sql
SELECT item_purchased,
ROUND(AVG(review_rating),2) AS avg_review_rating
FROM consumer
GROUP BY item_purchased
ORDER BY avg_review_rating DESC
LIMIT 5;
```

### Subscriber vs Non-Subscriber Spending

```sql
SELECT subscription_status,
COUNT(customer_id) AS total_customers,
SUM(purchase_amount) AS total_revenue,
AVG(purchase_amount) AS avg_spent
FROM consumer
GROUP BY subscription_status;
```

### Customer Segmentation

```sql
CASE
WHEN previous_purchases = 1 THEN 'New'
WHEN previous_purchases BETWEEN 2 AND 10 THEN 'Returning'
ELSE 'Loyal'
END AS customer_segment
```

---

# Power BI Dashboard

The Power BI dashboard provides an interactive overview of consumer behavior.

### Key Performance Indicators (KPIs)

| Metric              | Value  |
| ------------------- | ------ |
| Total Customers     | 3.9K   |
| Total Revenue       | $233K  |
| Average Rating      | 3.75   |
| Avg Purchase Amount | $59.76 |

### Dashboard Visualizations

* Customer Subscription Distribution
* Revenue by Category
* Sales by Category
* Revenue by Age Group
* Interactive Filters (Gender, Category, Shipping Type)

---

# Key Insights

### Customer Behavior

* Most customers are **non-subscribers**
* Repeat buyers contribute a significant portion of revenue

### Product Performance

* **Clothing category generates the highest revenue**
* Accessories and footwear show moderate demand

### Shipping Preferences

* Faster shipping methods often correlate with **higher purchase amounts**

### Customer Segmentation

Customers can be segmented into:

| Segment   | Description       |
| --------- | ----------------- |
| New       | First-time buyers |
| Returning | 2–10 purchases    |
| Loyal     | Frequent buyers   |

---

# Business Recommendations

### Increase Subscription Adoption

Offer incentives such as:

* exclusive discounts
* early product access
* free shipping

### Focus on High Revenue Categories

Invest marketing budget in:

* Clothing
* Accessories

### Improve Customer Retention

Implement:

* loyalty programs
* personalized product recommendations

### Improve Low-Rated Products

Analyze customer feedback and improve product quality.

---

# Tools & Technologies

| Tool     | Purpose                       |
| -------- | ----------------------------- |
| Python   | Data cleaning & preprocessing |
| Pandas   | Data manipulation             |
| NumPy    | Numerical operations          |
| MySQL    | Data analysis queries         |
| Power BI | Dashboard & visualization     |

---

# Project Structure

```
consumer-shopping-behavior-analysis
│
├── dataset
│   └── customer_shopping_behavior.csv
│
├── python
│   └── data_cleaning.ipynb
│
├── sql
│   └── analysis_queries.sql
│
├── dashboard
│   └── powerbi_dashboard.pbix
│
└── README.md
```

---

# Conclusion

This project demonstrates a **complete data analytics workflow**, starting from raw data cleaning to generating business insights using SQL and visualization tools.

The analysis helps businesses understand:

* customer purchasing patterns
* revenue distribution
* product performance
* customer segmentation

These insights can support **data-driven decision-making in retail businesses**.

---

# Author

**Sumit Gupta**

Aspiring **Data Analyst | Python | SQL | Power BI**

---
