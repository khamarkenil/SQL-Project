Question 1: find the total number of unique visitors (`fullVisitorID`)

SQL Queries: 
SELECT COUNT(DISTINCT full_visitorid) AS unique_visitors
FROM all_sessions;

Answer: 




Question 2: ind the total number of unique visitors by referring sites

SQL Queries:

SELECT
    channel_grouping AS referring_site,
    COUNT(DISTINCT full_visitorid) AS unique_visitors
FROM all_sessions
GROUP BY channel_grouping
ORDER BY unique_visitors DESC;


Answer:





Question 3: ind each unique product viewed by each visitor

SQL Queries:

SELECT DISTINCT
    full_visitorid AS visitor_id,
    product_sku AS product_id,
    v2_product_name AS product_name
FROM all_sessions
WHERE full_visitorid IS NOT NULL
    AND product_sku IS NOT NULL
    AND v2_product_name IS NOT NULL
    AND v2_product_name != '(not set)'
ORDER BY visitor_id, product_id;


Answer:





Question 4: compute the percentage of visitors to the site that actually makes a purchase


SQL Queries:

SELECT
    (COUNT(DISTINCT CASE WHEN transaction_id IS NOT NULL THEN full_visitorid END) * 100.0 / 
     COUNT(DISTINCT full_visitorid)) AS purchase_percentage
FROM all_sessions;

   
Answer:




Question 5: What is the average revenue per purchase for each product category

SQL Queries:

SELECT
    v2_product_category AS product_category,
    SUM((product_price * product_quantity) / 1000000.0) / COUNT(DISTINCT transaction_id) AS avg_revenue_per_purchase
FROM all_sessions
WHERE transaction_id IS NOT NULL
    AND product_price IS NOT NULL
    AND product_quantity IS NOT NULL
    AND v2_product_category IS NOT NULL
    AND v2_product_category != '(not set)'
GROUP BY v2_product_category
ORDER BY avg_revenue_per_purchase DESC;


Answer:
