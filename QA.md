What are your risk areas? Identify and describe them.

There are multiple risk areas which I identify in the give datasets.

1. Data Consistency: Columns might have inconsistent values or data type which can lead into getting error which sorting or grouping by.

2. Data Completeness: There can be multiple records where the value is NULL which could affect the output.

3.Duplicate Records: Duplicate values might exist because of incorrectly logging error, inflating revenue or counts of product sold.

4.Aggregation Errors: Errors in SQL aggregation (e.g., SUM, COUNT, GROUP BY) due to incorrect handling of nulls, duplicates, or joins can produce wrong totals



QA Process:
Describe your QA process and include the SQL queries used to execute it.

- Firstly, I try look into data consistent by checking the product price is not in negative or has a correct format.

SELECT
    MIN(product_price) AS min_price,
    MAX(product_price) AS max_price,
    AVG(product_price) AS avg_price
FROM all_sessions
WHERE product_price IS NOT NULL
    AND transaction_id IS NOT NULL;


- Secondly, I tried looking for any null values if any to asses its impact on queries.

SELECT
    COUNT(*) AS total_rows,
    COUNT(*) - COUNT(full_visitorid) AS null_visitor_id,
    COUNT(*) - COUNT(product_sku) AS null_product_sku,
    COUNT(*) - COUNT(transaction_id) AS null_transaction_id,
    COUNT(*) - COUNT(product_price) AS null_product_price,
    COUNT(*) - COUNT(product_quantity) AS null_product_quantity,
    COUNT(*) - COUNT(v2_product_category) AS null_product_category,
    SUM(CASE WHEN v2_product_category = '(not set)' THEN 1 ELSE 0 END) AS not_set_product_category,
    SUM(CASE WHEN city = 'not available in demo dataset' THEN 1 ELSE 0 END) AS not_available_city
FROM all_sessions;

-Thirdly, I look for duplicate records for multiple rows.

SELECT
    full_visitorid,
    visit_id,
    product_sku,
    transaction_id,
    COUNT(*) AS record_count
FROM all_sessions
WHERE transaction_id IS NOT NULL
GROUP BY full_visitorid, visit_id, product_sku, transaction_id
HAVING COUNT(*) > 1
ORDER BY record_count DESC;