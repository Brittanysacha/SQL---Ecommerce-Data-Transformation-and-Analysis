What are your risk areas? Identify and describe them.



QA Process:
Describe your QA process and include the SQL queries used to execute it.

<!-- The QA process overlaps with the data cleaning process, in that it aims to ensure that data is accurate, consistent, and complete. Although, some of these codes have been ran during the data cleaning process, they have been broken down into some steps below. -->

<!-- Check number of records to make sure that all expected data is there. -->

SELECT COUNT(*) FROM all_sessions;
SELECT COUNT(*) FROM analytics;
SELECT COUNT(*) FROM products;
SELECT COUNT(*) FROM sales_by_sku;
SELECT COUNT(*) FROM sales_report;

<!-- Check for duplicate records in each table to ensure that data is not duplicated. This was completed during the data cleaning phase, using a 3-step query.  -->

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

<!-- Identify missing values within each table, and ensure that all required data is present (i.e. ID numbers): -->

SELECT COUNT(*) FROM all_sessions WHERE full_visitor_id IS NULL;
SELECT COUNT(*) FROM analytics WHERE visit_id IS NULL;
SELECT COUNT(*) FROM products WHERE sku IS NULL;
SELECT COUNT(*) FROM sales_by_sku WHERE product_sku IS NULL;
SELECT COUNT(*) FROM sales_report WHERE product_sku IS NULL;

<!-- Check for outliers within the data, that are 3 standard deviations away from the average. One example query is included below, however, this would need to be completed for all integers within each table.  -->

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

<!-- Unidentified outliers can skew analysis results, which can lead to incorrect conclusions. However, duplicates can create bias within analysis results, and also lead to correct conclusions. What step you take first, should be determined by the overall goals of analysis. However, if removing outliers prior to duplicates, a further check for outliers needs to be completed after duplicates are removed. This is because the central mean that standard deviations are based on will have changed.   -->