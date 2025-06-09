# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

The goal of this project was to learn SQL by analyzing an e-commerce dataset to understand customer behaviour and sales patterns. I worked with five tables (all sessions, products, sales_by_sku, sales_report, analytics) to answer business related questions like how many unique visitors come to the site, what percentage of visitors make purchases, and which product categories generate the most revenue per purchase. 

My main objectives were:

Practice writing SQL queries to calculate metrics like visitor counts, revenue, and product views.
Clean and transform data to handle issues like missing values and duplicates.
Explore patterns in customer behavior, such as top-selling products and popular referral sources.
Develop a quality assurance (QA) process to ensure my query results were accurate.

## Process
### Step 1: Understand the Datasets
I started by creating table and importing data to postgreSQL.  Than I explored the datasets, focusing on all sessions, which contains session data like visitor IDs (fullVisitorId), product SKUs (productSKU), transaction IDs (transactionId), product prices (productPrice), and categories (v2ProductCategory). 

I tried using simple queries to check column names and sample data to better understand each table what kind of information the each table has for me to work on.

### Step 2: Clean data and Write SQL Queries to Answer Questions

I noticed issues like null values and placeholders (e.g., '(not set)' in v2ProductCategory). I added WHERE clauses to filter them:
WHERE fullVisitorId IS NOT NULL 
AND v2ProductCategory != '(not set)';

I also learned to handle productPrice in micro-units (e.g., 19990000 = $19.99) by dividing by 1000000.0.

I wrote queries to answer specific questions about the data, with basic SQL concepts.

Unique Visitors: Counted unique visitors using COUNT(DISTINCT fullVisitorId):
SELECT COUNT(DISTINCT fullVisitorId) AS unique_visitors 
FROM all_sessions;


## Results

This data analysis provided several insights:

Visitor Traffic: The site had thousands of unique visitors.
Purchase Behavior: Only about 2–3% of visitors made purchases.
Revenue Patterns: The U.S. generated the most revenue, with city like San Francisco. Electronics (e.g., Nest products) had the highest average revenue per purchase ($250+), while T-shirts and journals were lower ($20–$25).
Product Views: Visitors viewed a variety of products, with T-shirts and electronics being popular across regions.
These findings helped me understand which products and regions brings more sales and how traffic sources influence purchases.



## Challenges 

I faced several challenges:

Understanding Data: It was hard to know what columns like channel Grouping or transaction Id meant at first. I used SELECT DISTINCT queries to explore values.
Handling Nulls: Many rows had null transactionId or city values which made some analyses less accurate. I learned to filter them with WHERE.
Duplicates: Finding duplicates was tricky. My first query had an error (HAVING order_count didn’t work), but I learned to use HAVING COUNT(*) > 1.
Query Complexity: Writing queries with DISTINCT or CASE was confusing at first, but breaking them into steps (e.g., using CTEs) helped.


## Future Goals

If I had more time, I would have tried to do below things:

Join Datasets: Combine all_sessions.csv with products.csv to analyze product sentiment or stock levels alongside sales.
Analyze Time Trends: Use date or timeOnsSite to study seasonal patterns or session durations, but the 2016–2017 data limited this.
Improve QA: Create more automated checks (e.g., scripts to flag inconsistencies) to catch errors faster.
