Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT 
    country,
    city,
    SUM(CASE 
        WHEN total_transaction_revenue is NULL THEN 0 
        ELSE CAST(total_transaction_revenue AS NUMERIC) / 1000000 
    END) AS total_revenue
FROM all_sessions
WHERE city NOT IN ('unknown', '(not set)', 'not available in demo dataset')
GROUP BY country, city
ORDER BY total_revenue DESC
LIMIT 10;



Answer:






**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

SELECT 
    country,
    city,
    AVG(product_quantity) AS avg_products_ordered
FROM all_sessions
WHERE city NOT IN ('unknown', '(not set)', 'not available in demo dataset')
  AND product_quantity IS NOT NULL
GROUP BY country, city
HAVING COUNT(product_quantity) > 0
ORDER BY avg_products_ordered DESC
LIMIT 10;




Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

SELECT
    country,
    city,
    v2_product_category AS product_category,
    COUNT(*) AS order_count,
    SUM(CASE WHEN product_quantity IS NOT NULL THEN product_quantity ELSE 1 END) AS total_quantity_ordered
FROM all_sessions
WHERE product_sku IS NOT NULL
    AND v2_product_category IS NOT NULL
    AND v2_product_category != '(not set)'
GROUP BY country, city, v2_product_category
HAVING COUNT(*) > 0
ORDER BY country, city, order_count DESC, total_quantity_ordered DESC;


Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

WITH product_sales AS (
    SELECT
        country,
        city,
        product_sku,
        v2_product_name AS product_name,
        SUM(CASE WHEN product_quantity IS NOT NULL THEN product_quantity ELSE 1 END) AS total_quantity_sold
    FROM all_sessions
    WHERE product_sku IS NOT NULL
        AND v2_product_name IS NOT NULL
        AND v2_product_name != '(not set)'
    GROUP BY country, city, product_sku, v2_product_name
),
RankedProducts AS (
    SELECT
        country,
        city,
        product_sku,
        product_name,
        total_quantity_sold,
        ROW_NUMBER() OVER (PARTITION BY country, city ORDER BY total_quantity_sold DESC, product_name) AS rn
    FROM product_sales
)
SELECT
    country,
    city,
    product_sku,
    product_name,
    total_quantity_sold
FROM RankedProducts
WHERE rn = 1
ORDER BY country, city, total_quantity_sold DESC;


Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

SELECT
    a.country,
    a.city,
    a.v2_product_category AS product_category,
    SUM(CASE 
        WHEN a.product_price IS NOT NULL AND a.product_quantity IS NOT NULL 
        THEN a.product_price * a.product_quantity 
        ELSE 0 
    END) / 1000000 AS total_revenue
FROM all_sessions a
LEFT JOIN products p ON a.product_sku = p.SKU
WHERE a.product_sku IS NOT NULL
GROUP BY a.country, a.city, a.v2_product_category
ORDER BY total_revenue DESC;


Answer:







