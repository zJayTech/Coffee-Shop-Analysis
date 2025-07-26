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

### 1. Total Sales Revenue
Problem: Calculate the total revenue generated from all coffee sales. This query helps understand the overall financial performance.
```sql
SELECT
    SUM(CAST(REPLACE(money, 'R', '') AS DECIMAL(10, 2))) AS TotalRevenue
FROM
    coffee_sales;
```
### Explanation:
REPLACE(money, 'R', ''): This function removes the 'R' prefix from the money column, converting 'R38.70' to '38.70'.

CAST(... AS DECIMAL(10, 2)): This converts the cleaned string value to a decimal number with 10 total digits and 2 decimal places, suitable for accurate monetary calculations.

SUM(...): This aggregates all the converted money values to provide the total sales revenue.

---

### 2. Top 5 Best-Selling Coffee Products
Problem: Identify the top 5 coffee products based on the number of units sold. This helps in understanding product popularity and inventory management.
```sql
SELECT TOP 5
    coffee_name,
    COUNT(*) AS NumberOfSales
FROM
    coffee_sales
GROUP BY
    coffee_name
ORDER BY
    NumberOfSales DESC;
```
### Explanation:
COUNT(*): Counts the occurrences of each coffee_name to determine sales volume.

GROUP BY coffee_name: Groups the rows by coffee_name so that COUNT(*) calculates the sales for each unique coffee product.

ORDER BY NumberOfSales DESC: Sorts the results in descending order based on the NumberOfSales to bring the best-selling products to the top.

SELECT TOP 5: Limits the output to only the top 5 rows after sorting.

---

### 3. Sales Trend by Time of Day
Problem: Analyze how sales vary throughout different times of the day (Morning, Afternoon, Evening). This can inform staffing and marketing strategies.
```sql
SELECT
    Time_of_Day,
    COUNT(*) AS NumberOfSales,
    SUM(CAST(REPLACE(money, 'R', '') AS DECIMAL(10, 2))) AS TotalRevenue
FROM
    coffee_sales
GROUP BY
    Time_of_Day
ORDER BY
    CASE
        WHEN Time_of_Day = 'Morning' THEN 1
        WHEN Time_of_Day = 'Afternoon' THEN 2
        WHEN Time_of_Day = 'Evening' THEN 3
        ELSE 4
    END;
```
### Explanation:
COUNT(*): Calculates the number of sales for each Time_of_Day.

SUM(CAST(REPLACE(money, 'R', '') AS DECIMAL(10, 2))): Calculates the total revenue for each Time_of_Day.

GROUP BY Time_of_Day: Groups the results by Time_of_Day to aggregate sales and revenue for each period.

ORDER BY CASE WHEN Time_of_Day = 'Morning' THEN 1 ... END: Orders the results logically by Time_of_Day (Morning, Afternoon, Evening) rather than alphabetically.

---

### 4. Sales Trend by Weekday
Problem: Determine which weekdays have the highest or lowest sales. This insight is useful for planning promotions or staffing adjustments.
```sql
SELECT
    Weekday,
    COUNT(*) AS NumberOfSales,
    SUM(CAST(REPLACE(money, 'R', '') AS DECIMAL(10, 2))) AS TotalRevenue
FROM
    coffee_sales
GROUP BY
    Weekday, Weekdaysort
ORDER BY
    Weekdaysort;
```
### Explanation:
COUNT(*): Counts the total sales for each Weekday.

SUM(CAST(REPLACE(money, 'R', '') AS DECIMAL(10, 2))): Calculates the total revenue for each Weekday.

GROUP BY Weekday, Weekdaysort: Groups the data by Weekday and Weekdaysort to ensure correct aggregation.

ORDER BY Weekdaysort: Sorts the results by the numerical representation of the weekday (e.g., Monday=1, Tuesday=2), ensuring the days are ordered correctly.

---

### 5. Average Transaction Value
Problem: Calculate the average amount of money spent per transaction. This metric can help assess customer spending habits.
```sql
SELECT
    AVG(CAST(REPLACE(money, 'R', '') AS DECIMAL(10, 2))) AS AverageTransactionValue
FROM
    coffee_sales;
```
### Explanation:
REPLACE(money, 'R', ''): Removes the 'R' prefix from the money column.

CAST(... AS DECIMAL(10, 2)): Converts the cleaned string to a decimal number.

AVG(...): Calculates the average of these converted monetary values across all transactions.

---

### 6. Monthly Sales Over Time
Problem: Track total sales revenue month by month to identify seasonal trends or long-term growth.
```sql
SELECT
    Month_name,
    Monthsort,
    SUM(CAST(REPLACE(money, 'R', '') AS DECIMAL(10, 2))) AS TotalMonthlyRevenue
FROM
    coffee_sales
GROUP BY
    Month_name, Monthsort
ORDER BY
    Monthsort;
```
### Explanation:
SUM(CAST(REPLACE(money, 'R', '') AS DECIMAL(10, 2))): Calculates the total revenue for each month.

GROUP BY Month_name, Monthsort: Groups the data by Month_name and Monthsort to aggregate sales for each month.

ORDER BY Monthsort: Sorts the results by the numerical representation of the month to ensure chronological order.

---

### Demonstrated Skills
Data Analysis: Ability to understand a dataset's structure and content, identify key questions, and extract meaningful insights.

* SQL (Microsoft SQL Server): Proficiency in writing and executing complex SQL queries, including:

* Data Retrieval: Selecting specific columns and applying filtering conditions.

* Aggregate Functions: Using SUM(), COUNT(), and AVG() for data summarization.

* Data Manipulation: Applying string functions like REPLACE() for data cleaning and transformation.

* Type Casting: Converting data types (e.g., VARCHAR to DECIMAL) for accurate calculations.

* Grouping and Ordering: Utilizing GROUP BY and ORDER BY clauses to aggregate and sort results effectively.

* Top N Queries: Using SELECT TOP N to retrieve the highest or lowest values.

Data Cleaning and Transformation: Experience in preparing raw data for analysis by handling inconsistencies and converting data types.

Problem Solving: Capacity to translate business questions into actionable data queries.

### Dataset Credit
Dataset by reignrichard on Kaggle
Format: CSV | Contains: product names, transaction IDs, payment types, timestamps, and revenue
