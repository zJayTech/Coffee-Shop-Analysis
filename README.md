# Coffee Store Sales Analysis

### Project Objective:
To conduct a comprehensive analysis of coffee sales data, aiming to identify top-selling products, uncover sales patterns by time of day and weekday, and evaluate overall revenue performance.

Using Microsoft SQL Server, this project highlights the use of SQL for data cleaning, transformation, and advanced querying to extract meaningful business insights and reveal key performance trends.

### Dataset:
**Source**: [Coffee Store Sales – Kaggle](https://www.kaggle.com/datasets/reignrichard/coffee-store-sales?resource=download)

> *The dataset was saved as a .csv file and named "coffee_sales" before being imported into Microsoft SQL Server*

The dataset contains detailed transaction records including sales date, time, payment type, product name, and sales amount.

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

## SQL Queries for Coffee Sales Data Analysis
This section outlines several SQL queries designed to extract valuable insights from the coffee_sales dataset.

### 1. Total Sales Revenue
__Problem:__ Calculate the total revenue generated from all coffee sales. This query helps understand the overall financial performance.
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


### Result:
| TotalRevenue |
|--------------|
| 115431.58    |

---

### 2. Top 5 Best-Selling Coffee Products
__Problem:__ Identify the top 5 coffee products based on the number of units sold. This helps in understanding product popularity and inventory management.
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


### Result:
| coffee_name           | NumberOfSales   |
|-----------------------|-----------------|
| Americano with Milk   | 824             |
| Latte                 | 782             |
| Americano             | 578             |
| Cappuccino            | 501             |
| Cortado               | 292             |

---

### 3. Sales Trend by Time of Day
__Problem:__ Analyze how sales vary throughout different times of the day (Morning, Afternoon, Evening). This can inform staffing and marketing strategies.
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


### Result:
| Time_of_Day   | NumberOfSales   | TotalRevenue   |
|---------------|-----------------|----------------|
| Morning       | 1221            | 37380.20       |
| Afternoon     | 1231            | 39018.04       |
| Night         | 1184            | 39033.34       |

---

### 4. Sales Trend by Weekday
__Problem:__ Determine which weekdays have the highest or lowest sales. This insight is useful for planning promotions or staffing adjustments.
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


### Result:
| Weekday  | NumberOfSales  | TotalRevenue  |
|----------|----------------|---------------|
| Mon      | 561            | 17925.10      |
| Tue      | 585            | 18637.38      |
| Wed      | 510            | 16093.46      |
| Thu      | 520            | 16477.40      |
| Fri      | 544            | 17257.66      |
| Sat      | 482            | 15182.52      |
| Sun      | 434            | 13858.06      |

---

### 5. Average Transaction Value
__Problem:__ Calculate the average amount of money spent per transaction. This metric can help assess customer spending habits.
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


### Result:
| AverageTransactionValue  |
|--------------------------|
| 31.746859                |

---

### 6. Monthly Sales Over Time
__Problem:__ Track total sales revenue month by month to identify seasonal trends or long-term growth.
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


### Result:
| Month_name  | Monthsort   | TotalMonthlyRevenue  |
|-------------|-------------|----------------------|
| Jan         | 1           | 6398.86              |
| Feb         | 2           | 13215.48             |
| Mar         | 3           | 17036.64             |
| Apr         | 4           | 6720.56              |
| May         | 5           | 9063.42              |
| Jun         | 6           | 7758.76              |
| Jul         | 7           | 6915.94              |
| Aug         | 8           | 7613.84              |
| Sep         | 9           | 9988.64              |
| Oct         | 10          | 13891.16             |
| Nov         | 11          | 8590.54              |
| Dec         | 12          | 8237.74              |

---

### Key Tasks & Methodology:
* __Data Inspection & Understanding:__ Initial examination of the dataset to comprehend its structure, columns, and data types.

* __Data Cleaning & Transformation:__ Preparation of raw data for analysis, specifically converting monetary values (e.g., 'R38.70' to DECIMAL) and handling date/time fields for proper aggregation.

* __Problem Formulation:__ Identifying relevant business questions that could be answered through SQL queries, such as "What is the total revenue?", "Which are the best-selling coffee products?", and "How do sales vary by weekday or time of day?".

* __SQL Query Development:__ Crafting efficient and effective SQL queries for Microsoft SQL Server to address each business question. Queries involved the use of aggregate functions (SUM, COUNT, AVG), string manipulation (REPLACE), type casting (CAST), grouping (GROUP BY), and ordering (ORDER BY).

* __Insight Extraction:__ Interpreting query results to derive meaningful insights into sales patterns and product performance.

### Demonstrated Skills
* __Data Analysis:__ Ability to interpret datasets, identify analytical goals, and derive actionable conclusions.

* __SQL (Microsoft SQL Server):__ Advanced querying skills, including data manipulation (cleaning, casting), aggregation, filtering, and sorting to answer specific business questions.

* __Data Cleaning & Transformation:__ Practical experience in preparing messy data for analytical purposes.

* __Problem Solving:__ Translating business requirements into technical solutions using database queries.

* __Documentation:__ Clear and concise explanation of analytical approaches and SQL query logic.

### Outcome:
This project successfully demonstrates the ability to leverage SQL for insightful data analysis, providing a clear picture of coffee sales performance, identifying popular products, and highlighting crucial sales trends over time and by daily/weekly periods.

### Dataset Credit
Dataset by reignrichard on Kaggle
Format: CSV | Contains: product names, transaction IDs, payment types, timestamps, and revenue

---

## Contact

Feel free to reach out if you have any questions or feedback!

### Zack Jones
* **Email:** zackjones.dataanalyst@gmail.com
* **LinkedIn:** www.linkedin.com/in/zjaytech/

