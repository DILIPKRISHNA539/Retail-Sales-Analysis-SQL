# 🛍️ Retail Sales Analysis using SQL

## 📌 Project Overview
- This project demonstrates SQL skills by simulating the work of a Data Analyst exploring, cleaning, and analyzing retail sales data. You’ll set up a relational database, run exploratory data analysis (EDA), and generate business insights using SQL.

---

## 🎯 Objectives
- ✅ Set up and populate a retail sales database
- ✅ Clean data by identifying and removing nulls
- ✅ Perform exploratory data analysis
- ✅ Answer key business questions through SQL queries

---

## 🗂️ Database & Table Setup

```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales (
    transactions_id INT PRIMARY KEY,
    sale_date DATE,
    sale_time TIME,
    customer_id INT,
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,
    cogs FLOAT,
    total_sale FLOAT
);

 Data Exploration & Cleaning
sql
Copy
Edit
-- Total records
SELECT COUNT(*) FROM retail_sales;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Unique categories
SELECT DISTINCT category FROM retail_sales;

-- Null value check
SELECT * FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR
      gender IS NULL OR age IS NULL OR category IS NULL OR
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Remove rows with nulls
DELETE FROM retail_sales
WHERE sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR
      gender IS NULL OR age IS NULL OR category IS NULL OR
      quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
📊 Business Questions & Insights
🔹 Sales on a specific date
sql
Copy
Edit
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
🔹 Clothing category with quantity > 4 in Nov-2022
sql
Copy
Edit
SELECT * FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity > 4;
🔹 Total sales by category
sql
Copy
Edit
SELECT category, SUM(total_sale) AS net_sale, COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;
🔹 Avg. age of Beauty product customers
sql
Copy
Edit
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
🔹 High-value transactions (sale > 1000)
sql
Copy
Edit
SELECT * FROM retail_sales
WHERE total_sale > 1000;
🔹 Transactions count by gender & category
sql
Copy
Edit
SELECT category, gender, COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category;
🔹 Best selling month in each year
sql
Copy
Edit
SELECT year, month, avg_sale
FROM (
  SELECT 
    EXTRACT(YEAR FROM sale_date) AS year,
    EXTRACT(MONTH FROM sale_date) AS month,
    AVG(total_sale) AS avg_sale,
    RANK() OVER (PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
  FROM retail_sales
  GROUP BY 1, 2
) AS t1
WHERE rank = 1;
🔹 Top 5 highest spending customers
sql
Copy
Edit
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
🔹 Unique customers per category
sql
Copy
Edit
SELECT category, COUNT(DISTINCT customer_id) AS cnt_unique_cs
FROM retail_sales
GROUP BY category;
🔹 Orders by time shift
sql
Copy
Edit
WITH hourly_sale AS (
  SELECT *,
    CASE
      WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
      WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
      ELSE 'Evening'
    END AS shift
  FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
📈 Key Findings
Customer Demographics: Wide age range across gender.

High-Value Orders: Multiple transactions over $1000 show potential premium customer segment.

Sales Trends: Month-over-month comparison helps track peak seasons.

Top Customers: Power users identified for targeted marketing.

Popular Categories: Clothing and Beauty top the charts.

🧾 Reports
Sales Summary: Total sales and category performance

Trends: Monthly & shift-based sales distribution

Customer Insights: Power users and category-wise engagement

✅ Conclusion
This project is a solid introduction to SQL for aspiring data analysts. It walks through real-world analysis steps including setting up a database, cleaning data, and running analytical queries to answer business questions.

