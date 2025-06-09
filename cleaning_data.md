What issues will you address by cleaning the data?

- Data Consistency
- Data Completeness
- Duplicate Records




Queries:
Below, provide the SQL queries you used to clean your data.


-- let me fix the unit cost values first


UPDATE	analytics
SET 	unit_price = unit_price / 1000000;


-- removing duplicate values now.

DELETE FROM all_sessions
WHERE ctid NOT IN (
  SELECT MIN(ctid)
  FROM all_sessions
  GROUP BY 
    full_visitor_id, channel_grouping, time, country, city, total_transaction_revenue,
    transactions, time_on_site, page_views, session_quality_dim, date, visit_id, type,
	product_refund_amount, product_quantity, product_price, product_revenue, product_sku,
	v2_product_name, v2_product_category, product_variant, currency_code, item_quantity,
	item_revenue, transaction_revenue, transaction_id, page_title, search_keyword, page_path_level1, 
	ecommerce_action_type, ecommerce_action_step, ecommerce_action_option
);


DELETE FROM analytics
WHERE ctid NOT IN (
  SELECT MIN(ctid)
  FROM analytics
  GROUP BY visit_number, visit_id, visit_start_time, date, full_visitor_id, user_id, channel_grouping,
  social_engagement_type, units_sold, page_views, time_on_site, bounces, revenue, unit_price
);




DELETE FROM products
WHERE ctid NOT IN (
  SELECT MIN(ctid)
  FROM products
  GROUP BY sku, product_name, ordered_quantity, stock_level,
   			restocking_lead_time, sentiment_score, sentiment_magnitude
);

DELETE FROM sales_by_sku
WHERE ctid NOT IN (
  SELECT MIN(ctid)
  FROM sales_by_sku
  GROUP BY product_sku, total_ordered
);


DELETE FROM sales_report
WHERE ctid NOT IN (
  SELECT MIN(ctid)
  FROM sales_report
  GROUP BY product_sku, total_ordered, product_name, stock_level,
  			restocking_lead_time, sentiment_score, sentiment_magnitue, ratio
 
);



--  Delete all rows with null value, did not delete for all_sessions and analytics table and need to change type of the column





DELETE FROM products
WHERE sku IS NULL
   OR product_name IS NULL
   OR ordered_quantity IS NULL
   OR stock_level IS NULL
   OR restocking_lead_time IS NULL
   OR sentiment_score IS NULL
   OR sentiment_magnitude IS NULL;



DELETE FROM sales_by_sku
WHERE product_sku IS NULL
   OR sales_by_sku IS NULL;


DELETE FROM sales_report
WHERE product_sku IS NULL
   OR total_ordered IS NULL
   OR product_name IS NULL
   OR stock_level IS NULL
   OR restocking_lead_time IS NULL
   OR sentiment_score IS NULL
   OR sentiment_magnitue IS NULL
   OR ratio IS NULL;







