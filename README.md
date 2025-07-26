# Coffee Store Sales Analysis (SQL Portfolio Project)

This project explores transactional sales data from a coffee store using Microsoft SQL Server. The dataset includes timestamps, products, payment methods, and revenue information across thousands of transactions.

**Dataset Source**: [Coffee Store Sales – Kaggle](https://www.kaggle.com/datasets/reignrichard/coffee-store-sales?resource=download)

---

## Dataset Overview

| Column         | Description                                      |
|----------------|--------------------------------------------------|
| `date`         | Date of transaction                              |
| `datetime`     | Full timestamp                                   |
| `hour_of_day`  | Hour the transaction occurred                    |
| `cash_type`    | Payment method (cash or card)                    |
| `card`         | Anonymized card ID                               |
| `money`        | Transaction amount (in South African Rand)       |
| `coffee_name`  | Product sold                                     |
| `Time_of_Day`  | Time segment (Morning, Afternoon, etc.)          |
| `Weekday`      | Day of the week                                  |
| `Month_name`   | Month name                                       |
| `Weekdaysort`  | Weekday sort order                               |
| `Monthsort`    | Month sort order                                 |

---

## Analytical Questions and SQL Queries

### 1. What are the top 3 best-selling products by quantity sold?
```sql
SELECT TOP 3 product_name, COUNT(*) AS total_sold
FROM coffee_sales
GROUP BY product_name
ORDER BY total_sold DESC;
```
# Example in Context:
I group by product_name and count how many times each item was sold, ordering by volume. Limiting to the top 3 highlights the most popular products.

---

### 2. What is the peak hour for sales?
```sql
SELECT DATEPART(HOUR, time) AS sale_hour, COUNT(*) AS total_orders
FROM coffee_sales
GROUP BY DATEPART(HOUR, time)
ORDER BY total_orders DESC;
```
# Example in Context:
This query extracts the hour from each sale and counts the number of orders per hour. It helps identify when the store is busiest so staff or inventory can be adjusted accordingly.

---

### 3. Which product generated the highest total revenue?
```sql
SELECT product_name, SUM(amount) AS total_revenue
FROM coffee_sales
GROUP BY product_name
ORDER BY total_revenue DESC;
```
# Example in Context:
This query sums the amount for each product, revealing which item brings in the most money overall—not just popularity by volume.

---

### 4. How does daily revenue trend over time?
```sql
SELECT date, SUM(amount) AS daily_revenue
FROM coffee_sales
GROUP BY date
ORDER BY date;
```
# Example in Context:
Grouping by date and summing revenue lets us track how daily sales fluctuate. This could be visualized in a time series chart for trends and seasonality.

---

### Skills Demonstrated
* SQL data aggregation and grouping

* Window functions (e.g., SUM() OVER)

* Time-based filtering and analysis

* Data-driven business decision making

* Subqueries and performance metrics

### Dataset Credit
Dataset by reignrichard on Kaggle
Format: CSV | Contains: product names, transaction IDs, payment types, timestamps, and revenue
