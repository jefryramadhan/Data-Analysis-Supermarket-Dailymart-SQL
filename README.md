# Data Analysis Supermarket Dailymart with SQL

## Project Objective
This project demonstrate the ability of data end-to-end with replication previous analysis with Excel tools, now used SQL. The purpose is show the ability to handling larger data (-200.000 rows ) eficienly and used standard query database industry to generate same business insight.

## Data Used
- <a href=https://github.com/jefryramadhan/Data-Analysis-Supermarket-Dailymart-SQL/blob/main/Supermarket%20Data%20Analysist%20-%20SQL.csv>Supermarket Dailymart Data</a>

## Questions (KPIs)
- What is the total number of sales transactions in Jan - April 2025?
- How many unique customers came in January - April 2025?
- How many total products sold in January - April 2025?
- How many average item/product purchased by customers?
- How many verage weekly product sales?
- Which month did the biggest sales occur?
- What is the most sold product?

## Tools
* **Database:** MySQL (via Xampp)
* **SQL Client:** Visual Studio Code (VS Code)
* **Data Visualization:** Microsoft Excel (PivotTables & PivotCharts)

## Process
Proyek ini dikerjakan melalui beberapa tahapan utama:
1. **Data Preparation:** Imported Dataset CSV with -200.000 rows into MySQL Database use command `LOAD DATA LOCAL INFILE` to handling data raw.
2. **Data Transforming:** Function `STR_TO_DATE()` used in process import to ensuring format date its right and already to analyzed.
3. **Query & Analysis:** Each business questions(KPIs) answered with write the Query SQL Specifically use Clause `WHERE` to filtering, `GROUP BY` to Aggregation and functions like `COUNT(DISTINCT ...)`, `AVG()`, and `MONTHNAME()`.

## Query Key
```sql
-- 1. Counting the total unique transactions per Jan - April 2025
SELECT
    COUNT(DISTINCT TransactionID) AS Total_Transaction
FROM
    transactions;

-- 2. displaying the total unique Customers per Jan - April 2025
SELECT 
    COUNT(DISTINCT `CustomerID`) AS Total_Unique_Customer
FROM
    transactions;

-- 3. Displaying the total product sold per Jan - April 2025 
SELECT
    COUNT(`Products`) AS Total_Product_Sales
from 
    transactions;

-- 4. Counting Average item/product purchased by customers
Select 
    Round(COUNT(`TransactionID`) / COUNT(DISTINCT `TransactionID`), 3) AS Avg_Basket_Size
from 
    transactions;
 
-- 5. Displaying the total unique product
SELECT
    COUNT(DISTINCT `Products`) AS Total_Product
FROM 
    transactions;

-- 6. Counting average weekly product sales
WITH Sales_week AS (
    SELECT 
        YEARWEEK(Timestamp) AS Minggu,
        Count(TransactionID) AS Transaksi
    FROM 
        transactions
    GROUP BY
        Minggu
)
SELECT 
    ROUND(AVG(Transaksi), 2) AS Rata_Rata_Transaksi
FROM
    `Sales_week`;

-- 7. Displaying the month biggest sales
SELECT
    MONTHNAME(Timestamp) AS Bulan,
    COUNT(TransactionID) AS Transaksi
FROM
    transactions
WHERE
    'Timestamp' IS NOT NULL AND 'Timestamp' > '0000-00-00'
GROUP BY
    Bulan
ORDER BY
    Transaksi DESC
    LIMIT 1; 

-- 8. Displaying the most sold product
SELECT 
    `Products`,
    COUNT(TransactionID) AS Transaksi
FROM
    transactions
GROUP BY 
    `Products`
ORDER BY
    Transaksi DESC
    LIMIT 5 OFFSET 0;
