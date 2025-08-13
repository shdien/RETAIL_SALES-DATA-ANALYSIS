# **📌Project Overview**

This project demonstrates end-to-end retail sales data analysis using SQL.
It includes data cleaning, exploration, and analytical queries to generate valuable business insights such as top customers, best-selling months, sales by category, and shift-based order analysis.
The dataset represents transactional retail sales data containing sales dates, times, customer demographics, product categories, and sales amounts.

# **📂Table of Contents**

1. Dataset Structure

2. SQL Workflow

3. Key Analysis Performed

4. Technologies Used

5. How to Use

6. Insights Generated


# **⚙SQL Workflow**

**(1). Data Cleaning**

Identify and remove rows with NULL values.


**(2). Data Exploration**

Count total sales transactions.

Find unique customers.

List distinct product categories.


**(3). Data Analysis**

Filter sales by specific date, month, and category.

Calculate sales metrics such as:

Total sales & orders per category.

Average customer age by category.

High-value transactions (>1000).

Sales distribution by gender and category.

Best-selling month per year.

Top 5 customers by total sales.

Unique customers per category.

Shift-based transaction counts (Morning, Afternoon, Evening).


# **🔍Key Analysis Performed**

**Total Sales and Orders by Category**

'''

SELECT category,
       SUM(total_sale) AS sum_of_sale,
       COUNT(*) AS total_orders
FROM retail_sales
GROUP BY category;

'''

**Best-Selling Month Each Year**

'''

SELECT year, month, net_sale 
FROM (
    SELECT EXTRACT(YEAR FROM sale_date) AS year, 
           EXTRACT(MONTH FROM sale_date) AS month,
           AVG(total_sale) AS net_sale,
           RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS ranking
    FROM retail_sales
    GROUP BY 1,2
) AS t1
WHERE ranking = 1;

'''

**Shift-Wise Orders**

'''

WITH shift_table AS (
    SELECT *,
        CASE 
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift_time
    FROM retail_sales
)
SELECT shift_time, COUNT(*) AS total_transactions
FROM shift_table
GROUP BY shift_time;

'''

# **🛠Technologies Used**

SQL (PostgreSQL Syntax)

SQL Window Functions (RANK)

Aggregate Functions (SUM, AVG, COUNT)

Conditional Logic (CASE)

CTEs (WITH)






