<!-- What issues will you address by cleaning the data?
When cleaning the dataset, I wanted to check for a range of potential data issues including:

-	Outliers or extreme values
-   Missing or incomplete data
-	Duplicate entries
-	Incorrect data types
-	Inconsistent formatting or naming conventions -->

--------------------------------------------------------------------------------------------------------
<!-- To begin to address these issues, I wanted to perform a range of data cleaning tasks: -->

<!-- I started by executing statistical variance queries up to three degrees in the all_sessions table to identify outliers. I further imputed NULL for missing values. --> 

WITH as_missing_values_imputed AS (
  SELECT full_visitor_id, 
         time, 
         AVG(time) OVER (PARTITION BY full_visitor_id ORDER BY time) as imputed_time
  FROM all_sessions),
zscores AS (
  SELECT full_visitor_id,
         time,
         CASE 
           WHEN STDDEV(time) OVER (PARTITION BY full_visitor_id) = 0 THEN NULL 
           ELSE (time - imputed_time) / STDDEV(time) OVER (PARTITION BY full_visitor_id) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT full_visitor_id, time, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for all_sessions - time -->

WITH as_missing_values_imputed AS (
  SELECT full_visitor_id, 
         time_on_site, 
         AVG(time_on_site) OVER (PARTITION BY full_visitor_id ORDER BY time_on_site) as imputed_time_on_site
  FROM all_sessions
),
zscores AS (
  SELECT full_visitor_id,
         time_on_site,
         CASE 
           WHEN STDDEV(time_on_site) OVER (PARTITION BY full_visitor_id) = 0 THEN NULL 
           ELSE (time_on_site - imputed_time_on_site) / STDDEV(time_on_site) OVER (PARTITION BY full_visitor_id) 
         END as zscore
  FROM as_missing_values_imputed
)
SELECT full_visitor_id, time_on_site, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for all_sessions - time_on_site -->

WITH as_missing_values_imputed AS (
  SELECT full_visitor_id, 
         product_price, 
         AVG(product_price) OVER (PARTITION BY full_visitor_id ORDER BY product_price) as imputed_product_price
  FROM all_sessions),
zscores AS (
  SELECT full_visitor_id,
         product_price,
         CASE 
           WHEN STDDEV(product_price) OVER (PARTITION BY full_visitor_id) = 0 THEN NULL 
           ELSE (product_price - imputed_product_price) / STDDEV(product_price) OVER (PARTITION BY full_visitor_id) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT full_visitor_id, product_price, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for all_sessions - product_price -->

WITH as_missing_values_imputed AS (
  SELECT full_visitor_id, 
         page_views, 
         AVG(page_views) OVER (PARTITION BY full_visitor_id ORDER BY page_views) as imputed_page_views
  FROM all_sessions
),
zscores AS (
  SELECT full_visitor_id,
         page_views,
         CASE 
           WHEN STDDEV(page_views) OVER (PARTITION BY full_visitor_id) = 0 THEN NULL 
           ELSE (page_views - imputed_page_views) / STDDEV(page_views) OVER (PARTITION BY full_visitor_id) 
         END as zscore
  FROM as_missing_values_imputed
)
SELECT full_visitor_id, page_views, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for all_sessions - page_views -->


<!-- No outliers were found across the all_session table that warranted further investigation. Additionally, the columns total_transaction_revenue, transactions, session_quality_dim, product_refund_amount, product_quantity, product_revenue, item_quantity, transaction_revenue, transaction_id, search_keyword, and ecommerce_action_option all returned NULL values for all of their rows. Considering the goals of the data analysis, it would be appropriate to remove columns with no data that would not significantly impact the insights being sought. This would streamline the analysis and reduce the complexity of the data. -->

--------------------------------------------------------------------------------------------------------

<!-- I executed statistical variance queries up to three degrees to identify outliers in the products table.  I further imputed NULL for missing values. -->

WITH as_missing_values_imputed AS (
  SELECT sku, 
         ordered_quantity, 
         AVG(ordered_quantity) OVER (PARTITION BY sku ORDER BY ordered_quantity) as imputed_ordered_quantity
  FROM products),
zscores AS (
  SELECT sku,
         ordered_quantity,
         CASE 
           WHEN STDDEV(ordered_quantity) OVER (PARTITION BY sku) = 0 THEN NULL 
           ELSE (ordered_quantity - imputed_ordered_quantity) / STDDEV(ordered_quantity) OVER (PARTITION BY sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT sku, ordered_quantity, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for products - ordered_quantity -->

WITH as_missing_values_imputed AS (
  SELECT sku, 
         stock_level, 
         AVG(stock_level) OVER (PARTITION BY sku ORDER BY stock_level) as imputed_stock_level
  FROM products),
zscores AS (
  SELECT sku,
         stock_level,
         CASE 
           WHEN STDDEV(stock_level) OVER (PARTITION BY sku) = 0 THEN NULL 
           ELSE (stock_level - imputed_stock_level) / STDDEV(stock_level) OVER (PARTITION BY sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT sku, stock_level, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for products - stock_level -->

WITH as_missing_values_imputed AS (
  SELECT sku, 
         restocking_lead_time, 
         AVG(restocking_lead_time) OVER (PARTITION BY sku ORDER BY restocking_lead_time) as imputed_restocking_lead_time
  FROM products),
zscores AS (
  SELECT sku,
         restocking_lead_time,
         CASE 
           WHEN STDDEV(restocking_lead_time) OVER (PARTITION BY sku) = 0 THEN NULL 
           ELSE (restocking_lead_time - imputed_restocking_lead_time) / STDDEV(restocking_lead_time) OVER (PARTITION BY sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT sku, restocking_lead_time, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for products - restocking_lead_time -->

WITH as_missing_values_imputed AS (
  SELECT sku, 
         sentiment_score, 
         AVG(sentiment_score) OVER (PARTITION BY sku ORDER BY sentiment_score) as imputed_sentiment_score
  FROM products),
zscores AS (
  SELECT sku,
         sentiment_score,
         CASE 
           WHEN STDDEV(sentiment_score) OVER (PARTITION BY sku) = 0 THEN NULL 
           ELSE (sentiment_score - imputed_sentiment_score) / STDDEV(sentiment_score) OVER (PARTITION BY sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT sku, sentiment_score, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for products - sentiment_score -->

WITH as_missing_values_imputed AS (
  SELECT sku, 
         sentiment_magnitude, 
         AVG(sentiment_magnitude) OVER (PARTITION BY sku ORDER BY sentiment_magnitude) as imputed_sentiment_magnitude
  FROM products),
zscores AS (
  SELECT sku,
         sentiment_magnitude,
         CASE 
           WHEN STDDEV(sentiment_magnitude) OVER (PARTITION BY sku) = 0 THEN NULL 
           ELSE (sentiment_magnitude - imputed_sentiment_magnitude) / STDDEV(sentiment_magnitude) OVER (PARTITION BY sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT sku, sentiment_magnitude, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for products - sentiment_magnitude -->

<!-- No outliers were found across the products table that warranted further investigation. --> -->

--------------------------------------------------------------------------------------------------------

 <!-- I executed statistical variance queries up to three degrees to identify outliers in the sales_by_sku table.  I further imputed NULL for missing values. -->

WITH as_missing_values_imputed AS (
  SELECT product_sku, 
         total_ordered, 
         AVG(total_ordered) OVER (PARTITION BY product_sku ORDER BY total_ordered) as imputed_total_ordered
  FROM sales_by_sku),
zscores AS (
  SELECT product_sku,
         total_ordered,
         CASE 
           WHEN STDDEV(total_ordered) OVER (PARTITION BY product_sku) = 0 THEN NULL 
           ELSE (total_ordered - imputed_total_ordered) / STDDEV(total_ordered) OVER (PARTITION BY product_sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT product_sku, total_ordered, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for sales_by_sku - total_ordered  -->
<!-- No outliers were found across the sales_by_sku table that warranted further investigation

--------------------------------------------------------------------------------------------------------

<!-- I executed statistical variance queries up to three degrees to identify outliers in the analytics table.  I further imputed NULL for missing values. -->

WITH as_missing_values_imputed AS (
  SELECT visit_id, 
         visit_start_time, 
         AVG(visit_start_time) OVER (PARTITION BY visit_id ORDER BY visit_start_time) as imputed_visit_start_time
  FROM analytics),
zscores AS (
  SELECT visit_id,
         visit_start_time,
         CASE 
           WHEN STDDEV(visit_start_time) OVER (PARTITION BY visit_id) = 0 THEN NULL 
           ELSE (visit_start_time - imputed_visit_start_time) / STDDEV(visit_start_time) OVER (PARTITION BY visit_id) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT visit_id, visit_start_time, zscore
FROM zscores
WHERE zscore > 3;

<!-- Two outliers were found in the analytics dataset under the visit_start_time variable, both corresponding to the visit_id 1495607627. These outliers were found to be duplicated. It is recommended to remove the duplicate and further investigate and potentially remove the other outlier.-->

WITH as_missing_values_imputed AS (
  SELECT visit_id, 
        unit_price, 
         AVG(unit_price) OVER (PARTITION BY visit_id ORDER BY unit_price) as imputed_unit_price
  FROM analytics),
zscores AS (
  SELECT visit_id,
         unit_price,
         CASE 
           WHEN STDDEV(unit_price) OVER (PARTITION BY visit_id) = 0 THEN NULL 
           ELSE (unit_price - imputed_unit_price) / STDDEV(unit_price) OVER (PARTITION BY visit_id) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT visit_id, unit_price, zscore
FROM zscores
WHERE zscore > 3;

<!-- 13308 data entries were found to be outliers in the analytics dataset under the unit_price variable out of the total data entries (1048575). However, it is important to note that a significant amount of duplication exists within these outliers. Further investigation is recommended to determine the root cause of these outliers, which could be either issues with the data collection process or a large amount of variation among the analyzed variable. It is also recommended to test for outliers again once the duplicates have been removed. -->

<!-- The columns units_sold, revenue, user_id, and time_on_site, appearon first look to return NULL values for all of their rows. Considering the goals of the data analysis, it would be appropriate to remove columns with no data that would not significantly impact the insights being sought. This would streamline the analysis and reduce the complexity of the data. Further, the variable bounces appear to be constant. While it would be recommended to note its value, it would be better to remove it from the table to streamline the data and optimize efficiency and space. --> 

--------------------------------------------------------------------------------------------------------

 <!-- Finally, I examined the sales_report table. At first, the column titles seemed to be a duplicate of those in the previously queried products table. To investigate further, I executed a join query between these two tables  -->

SELECT *
FROM products
JOIN sales_report
ON products.sku = sales_report.product_sku;

<!-- "The query revealed that all column names, except for an extra ratio column in the sales_report table, were a match to those in the products table. To verify if the data was also duplicated, I conducted a further query to review duplication across tables." -->

SELECT sku, product_name, ordered_quantity, stock_level, restocking_lead_time, sentiment_score, sentiment_magnitude
FROM products
EXCEPT
SELECT product_sku, product_name, total_ordered, stock_level, restocking_leadtime, sentiment_score, sentiment_magnitude
FROM sales_report;

<!-- This revealed that there were 1009 entries within the products table that were not in the sales_report table. -->

SELECT product_sku, product_name, total_ordered, stock_level, restocking_leadtime, sentiment_score, sentiment_magnitude
FROM sales_report
EXCEPT 
SELECT sku, product_name, ordered_quantity, stock_level, restocking_lead_time, sentiment_score, sentiment_magnitude
FROM products;

<!-- This revealed that there were 371 entries in the sales_report table that were not in the products table. Therefore, I continued to execute statistical variance queries up to three degrees to identify outliers for all columns in the sales_report table with the expectation that i would need to review and remove duplications across the tables during the next phase of data cleaning.  -->

WITH as_missing_values_imputed AS (
  SELECT product_sku, 
         total_ordered, 
         AVG(total_ordered) OVER (PARTITION BY product_sku ORDER BY  total_ordered) as imputed_total_ordered
  FROM sales_report),
zscores AS (
  SELECT product_sku,
         total_ordered,
         CASE 
           WHEN STDDEV(total_ordered) OVER (PARTITION BY product_sku) = 0 THEN NULL 
           ELSE (total_ordered - imputed_total_ordered) / STDDEV(total_ordered) OVER (PARTITION BY product_sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT product_sku,  total_ordered, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for sales_report - total_ordered   -->

WITH as_missing_values_imputed AS (
  SELECT product_sku, 
         stock_level, 
         AVG(stock_level) OVER (PARTITION BY product_sku ORDER BY   stock_level) as imputed_stock_level
  FROM sales_report),
zscores AS (
  SELECT product_sku,
          stock_level,
         CASE 
           WHEN STDDEV(stock_level) OVER (PARTITION BY product_sku) = 0 THEN NULL 
           ELSE (stock_level - imputed_stock_level) / STDDEV(stock_level) OVER (PARTITION BY product_sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT  product_sku,  stock_level, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for sales_report - stock_level -->

WITH as_missing_values_imputed AS (
  SELECT product_sku, 
         restocking_leadtime, 
         AVG(restocking_leadtime) OVER (PARTITION BY product_sku ORDER BY restocking_leadtime) as imputed_restocking_leadtime
  FROM sales_report),
zscores AS (
  SELECT product_sku,
          restocking_leadtime,
         CASE 
           WHEN STDDEV(restocking_leadtime) OVER (PARTITION BY product_sku) = 0 THEN NULL 
           ELSE (restocking_leadtime - imputed_restocking_leadtime) / STDDEV(restocking_leadtime) OVER (PARTITION BY product_sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT  product_sku,  restocking_leadtime, zscore
FROM zscores
WHERE zscore > 3;
<!-- No outliers returned for sales_report - restocking_leadtime -->

WITH as_missing_values_imputed AS (
  SELECT product_sku, 
         sentiment_score, 
         AVG(sentiment_score) OVER (PARTITION BY product_sku ORDER BY  sentiment_score) as imputed_sentiment_score
  FROM sales_report),
zscores AS (
  SELECT product_sku,
         sentiment_score,
         CASE 
           WHEN STDDEV( sentiment_score) OVER (PARTITION BY product_sku) = 0 THEN NULL 
           ELSE (sentiment_score - imputed_sentiment_score) / STDDEV(sentiment_score) OVER (PARTITION BY product_sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT  product_sku, sentiment_score, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for sales_report - sentiment_score -->

WITH as_missing_values_imputed AS (
  SELECT product_sku, 
         sentiment_magnitude, 
         AVG(sentiment_magnitude) OVER (PARTITION BY product_sku ORDER BY  sentiment_magnitude) as imputed_sentiment_magnitude
  FROM sales_report),
zscores AS (
  SELECT product_sku,
         sentiment_magnitude,
         CASE 
           WHEN STDDEV(sentiment_magnitude) OVER (PARTITION BY product_sku) = 0 THEN NULL 
           ELSE (sentiment_magnitude - imputed_sentiment_magnitude) / STDDEV(sentiment_magnitude) OVER (PARTITION BY product_sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT  product_sku, sentiment_magnitude, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for sales_report - sentiment_magnitude -->

WITH as_missing_values_imputed AS (
  SELECT product_sku, 
         ratio, 
         AVG(ratio) OVER (PARTITION BY product_sku ORDER BY  ratio) as imputed_ratio
  FROM sales_report),
zscores AS (
  SELECT product_sku,
         ratio,
         CASE 
           WHEN STDDEV(ratio) OVER (PARTITION BY product_sku) = 0 THEN NULL 
           ELSE (ratio - imputed_ratio) / STDDEV(ratio) OVER (PARTITION BY product_sku) 
         END as zscore
  FROM as_missing_values_imputed)
SELECT  product_sku, ratio, zscore
FROM zscores
WHERE zscore > 3;

<!-- No outliers returned for sales_report - ratio. This is the only non-duplicated variable across the products and sales_report tables. -->

--------------------------------------------------------------------------------------------------------

<!-- The next step in the cleaning process is to remove non-significant variables with empty rows that cannot be calculated using available information. It is difficult to determine which rows should be calculated without knowing the purpose of the analysis or whether it's appropriate to request additional data from participants. 

As noted earlier, the variables total_transaction_revenue, transactions, session_quality_dim, product_refund_amount, product_quantity, product_revenue, item_revenue, item_quantity, transaction_revenue, transaction_id, search_keyword, and ecommerce_action_option from the all_sessions table appear to returned NULL values for all of their rows.  This was confirmed with the following queries below: -->

SELECT 'total_transaction_revenue' AS total_transaction_revenue, COUNT(DISTINCT total_transaction_revenue) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'transactions' AS transactions, COUNT(DISTINCT transactions) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'session_quality_dim' AS session_quality_dim, COUNT(DISTINCT session_quality_dim) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'product_refund_amount' AS product_refund_amount, COUNT(DISTINCT product_refund_amount) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'product_quantity' AS product_quality, COUNT(DISTINCT product_quantity) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'product_revenue' AS product_revenue, COUNT(DISTINCT product_revenue) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'item_revenue' AS item_revenue, COUNT(DISTINCT item_revenue) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'item_quantity' AS item_quantity, COUNT(DISTINCT item_quantity) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'transaction_revenue' As transactions_revenue, COUNT(DISTINCT transaction_revenue) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'transaction_id' AS transaction_id, COUNT(DISTINCT transaction_id) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'search_keyword' AS search_keyword, COUNT(DISTINCT search_keyword) AS distinct_count FROM all_sessions
UNION ALL
SELECT 'ecommerce_action_option' AS ecommerce_action_option, COUNT(DISTINCT ecommerce_action_option) AS distinct_count FROM all_sessions;

 <!-- The result showed five of the variables returned all NULL values. Since attempting to calculate these row values could potentially introduce additional errors or biases, I decided to remove them from the table using the DROP COLUMN query However, if an organization or business indicates that these rows are important, a different decision to calculate or retrieve this information may be more appropriate. -->

ALTER TABLE all_sessions 
    DROP COLUMN transactions, 
    DROP COLUMN product_refund_amount, 
    DROP COLUMN item_revenue,
    DROP COLUMN item_quantity, 
    DROP COLUMN search_keyword

<!-- The other variables had distinct values as follows: total_transaction_revenue (73), session_quality_dim (44), product_quantity (8), product_revenue (4), transaction_revenue (4), transaction_id (9),  ecommerce_action_option (3). I subsequently ran a COUNT IF NULL query to see the amount of NULL entries. -->

SELECT 
    COUNT(*) - COUNT(total_transaction_revenue) AS null_total_transaction_revenue,
    COUNT(*) - COUNT(transactions) AS null_transactions,
    COUNT(*) - COUNT(session_quality_dim) AS null_session_quality_dim,
    COUNT(*) - COUNT(product_refund_amount) AS null_product_refund_amount,
    COUNT(*) - COUNT(product_quantity) AS null_product_quantity,
    COUNT(*) - COUNT(product_revenue) AS null_product_revenue,
    COUNT(*) - COUNT(item_quantity) AS null_item_quantity,
    COUNT(*) - COUNT(transaction_revenue) AS null_transaction_revenue,
    COUNT(*) - COUNT(transaction_id) AS null_transaction_id,
    COUNT(*) - COUNT(search_keyword) AS null_search_keyword,
    COUNT(*) - COUNT(ecommerce_action_option) AS null_ecommerce_action_option
FROM all_sessions;

<!-- After running the query, it was evident that a high number of NULL values were present in each of the variables. The NULL values were observed across the following variables: total_transaction_revenue (15,053/15,134 rows), session_quality_dim (13,096/15,134 rows), product_quantity (15,081/15,134 rows), product_revenue (15,130/15,134 rows), transaction_revenue (15,130/15,134 rows), transaction_id (15,125/15,134 rows), and ecommerce_action_option (15,103/15,134 rows). The high number of NULL values across each of these variables makes the data incomplete. Therefore, I removed these variables using a DROP COLUMN query. I deemed this as appropriate because their presence could affect the accuracy of an analysis as it would be based off of a small sum of results. It is recommended to document the removal of these variables and investigate the data collection methods to understand why so little data was collected for these variables. This understanding can aid in the recollection of these variables if desired for future analysis. -->

<!-- The high number of NULL values across each of these variables makes the data incomplete. and would mean that analysis would likely not show a accurate representation of the data. Therefore, I decided to remove them from the table using a DROP COLUMN function. I would recomend notating the removal of these variables, and looking into data collection methods to determine why so little data was collected for these variables, so that it can be recollected if desired for future analysis. -->
ALTER TABLE all_sessions 
    DROP COLUMN total_transaction_revenue, 
    DROP COLUMN session_quality_dim, 
    DROP COLUMN product_quantity,
    DROP COLUMN product_revenue,
    DROP COLUMN transaction_revenue,
    DROP COLUMN transaction_id, 
    DROP COLUMN ecommerce_action_option;

--------------------------------------------------------------------------------------------------------

<!-- Additionally, the variables units_sold, revenue, user_id, and time_on_site from the analytics dataset appear to return NULL values for all of their rows. As well, the variable bounce variable appear to be constant. I confirmed this by running the following query below. -->

SELECT 'revenue' AS revenue, COUNT(DISTINCT revenue) AS distinct_count FROM analytics
UNION ALL
SELECT 'units_sold' AS units_sold, COUNT(DISTINCT units_sold) AS distinct_count FROM analytics
UNION ALL
SELECT 'bounce' AS bounce, COUNT(DISTINCT bounce) AS distinct_count FROM analytics
UNION ALL
SELECT 'user_id' AS user_id, COUNT(DISTINCT user_id) AS distinct_count FROM analytics
UNION ALL
SELECT 'time_on_site' AS time_on_site, COUNT(DISTINCT time_on_site) AS distinct_count FROM analytics;

<!-- The bounce column returned no unique values, which confirmed it was a constant variable. I therefore will remove it from the dataset. If important, the variable bounce should be noted either in a dataset description or the title of the chart. I will further remove the user_id variable as all returned variables showed as NULL. The other variables had distinct values as follows: revenue (1831), session_quality_dim (69). I will subsequently run a COUNT IF NULL query on the two variables to see the amount of NULL entries. -->

SELECT 
    COUNT(*) - COUNT(units_sold) AS null_units_sold,        
    COUNT(*) - COUNT(revenue) AS null_revenue,
    COUNT(*) - COUNT(time_on_site) AS null_time_on_site					
FROM analytics;

<!-- After running the query, it was evident that a high number of NULL values were present in both the units_sold and revenue variables. The NULL values were observed as follows: revenue (1044890/ 1048575 rows) and units_sold (1026595/1048575 rows). The high number of NULL values across both variables makes this data incomplete. Therefore, they were also removed using a DROP COLUMN function. The time_on_site variable had a lot less NULL values (120993/1048575), therefore it is recomended to keep NULL in place of missing values. However, to also look into the data collection process to see why this question is less frequently answered.  -->

ALTER TABLE analytics
    DROP COLUMN bounces
    DROP COLUMN units_sold
    DROP COLUMN revenue

 --------------------------------------------------------------------------------------------------------
<!-- The next step in the cleaning process is to remove all duplicates across each dataset. I removed duplicates from within tables, using three commands :  CREATE TABLE with SELECT DISTINCT, DROP TABLE, and ALTER TABLE.  -->

CREATE TABLE new_all_sessions AS
SELECT DISTINCT * FROM all_sessions;

DROP TABLE all_sessions;

ALTER TABLE new_all_sessions RENAME TO all_sessions;


CREATE TABLE new_analytics AS
SELECT DISTINCT * FROM analytics;

DROP TABLE analytics;

ALTER TABLE new_analytics RENAME TO analytics;


CREATE TABLE new_products AS
SELECT DISTINCT * FROM products;

DROP TABLE products;

ALTER TABLE new_products RENAME TO products;


CREATE TABLE new_sales_by_sku AS
SELECT DISTINCT * FROM sales_by_sku;

DROP TABLE sales_by_sku;

ALTER TABLE new_sales_by_sku RENAME TO sales_by_sku;


CREATE TABLE new_sales_report AS
SELECT DISTINCT * FROM sales_report;

DROP TABLE sales_report;

ALTER TABLE new_sales_report RENAME TO sales_report;

<!-- Having noted earlier that the sales_report and products tables held the same variables but contained some different values, I chose to join the like variables together using a CREATE TABLE and UNION command. Since the products table did not have a ratio variable, I needed to add one to the products table first so it could be a complete match for the sales_report table.  -->

ALTER TABLE products
CREATE COLUMN ratio DOUBLE PRECISION


CREATE TABLE new_sales_report AS
SELECT product_sku, total_ordered, product_name, stock_level, restocking_leadtime, sentiment_score, sentiment_magnitude, ratio
FROM sales_report
UNION
SELECT sku, ordered_quantity, product_name, stock_level, restocking_lead_time, sentiment_score, sentiment_magnitude, ratio
FROM products;

<!-- Afterwards I removed the initial products table and the sales_report, before running the same duplicate removal SELECT DISTINCT command, as previously done above. At that time renaming the table to product_sales_report.  -->

DROP TABLE sales_report;

DROP TABLE products;

CREATE TABLE products_sales_report AS
SELECT DISTINCT * FROM new_sales_report;

DROP TABLE new_sales_report;

<!-- The other duplicated table variables I became aware of were in the analytics and all_sessions tables. I tested this by matching three similar column variables.-->

SELECT analytics.visit_id, all_sessions.visit_id, analytics.date, all_sessions.date, 
analytics.channel_grouping, all_sessions.channel_grouping  
FROM analytics
JOIN all_sessions
ON  all_sessions.full_visitor_id = analytics.full_visitor_id;

<!-- The columns visit_id, date, and channel_grouping in the analytics and all_sessions tables had the same names but different values under each. There were also some unique variables in each table. Combining these tables would have resulted in a significant number of NULL values and incomplete data due to the differing values in their columns. Therefore, I recommend leaving the analytics and all_sessions tables separate for now. However, it may be worth considering how they can be combined for future data collection purposes. -->
------------------------------------------------------------------------------------------------------
<!-- The next step in the cleaning process is to fix any incorrect data types. I noticed that the unit_price column in the analytics table and the product_price column in the all_sessions table have values that appear to be in millions but should not be, as they should be representative of a dollar value. To correct this, I suggest dividing the values in these columns by 1,000,000 using the following SQL statements: -->

UPDATE analytics
SET unit_price = unit_price / 1000000;


UPDATE all_sessions
SET product_price = product_price / 1000000;

This will convert the values in these columns from a millionth of a unit to a unit, making them easier to interpret and analyze."

------
<!-- Inconsistent formatting or naming conventions


All variable titles with the SKU were updated to follow the format of word_word. If there was a similar variable formatted differently under a different table, it was corrected. All variable titles and table names were kept as lower case. -->

ALTER TABLE product_sales_report RENAME COLUMN restocking_leadtime TO restocking_lead_time;


