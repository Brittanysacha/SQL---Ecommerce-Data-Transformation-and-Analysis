What are your risk areas? Identify and describe them.
------------------------------------------------------------------------------------------------------------

The Ecommerce database has numerous areas of risk, which should be of concern. These include data accuracy, completeness, consistency, and validity issues such as duplication, incorrect numbering formats, inconsistent connecting variables across tables, and missing or NULL data. The data can be cleaned, with duplicates removed, formatting corrected, naming conventions standardized across tables, and missing data filled or imputed where needed. Nonetheless, even after resolving existing issues, the presence of inaccurate and inconsistent data across all of the datasets within the ecommerce database should raise concern about the accuracy and validity of other data that appears consistent and valid. 

Further, another risk that should be addressed during QA, is the imcomplete list og countries and cities included within the all_sessions table of the Ecommerce database. Although, it is possible that this particular Ecommerce organization does not sell products outside of the regions they included in the all_sessions dataset, there a large number of variables related to products sold without country or city information (imputted as NULL or not available in demo dataset). Without all countries and cities accounted for, there could be inaccurate data generated for reporting and insights. Particuarly if this data is used for market analysis. Comprehensive and up-to-date information on countries and cities would be reqired to minimize the risk of erroneous decisions 

------------------------------------------------------------------------------------------------------------
QA Process:
Describe your QA process and include the SQL queries used to execute it.
------------------------------------------------------------------------------------------------------------


The QA process overlaps with the data cleaning process, in that it aims to ensure that data is accurate, consistent, and complete. Although, some of these codes have been ran during the data cleaning process, they have been broken down into some steps below. 

I checked the number of data rows to make sure that all expected data copied over from the CSV files was there. 

SELECT COUNT(*) FROM all_sessions;
SELECT COUNT(*) FROM analytics;
SELECT COUNT(*) FROM sales_by_sku;
SELECT COUNT(*) FROM products_sales_report;


------------------------------------------------------------------------------------------------------------
I checked for duplicate records in each table to ensure that data is not duplicated. This was completed during the data cleaning phase, using a 3-step query.  -->

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


------------------------------------------------------------------------------------------------------------
I further compared data across tables, where there were similar variable names that were not associated with the primary key (I set my primary keys as product_sku and full_visitor_id).  -->

SELECT sku, product_name, ordered_quantity, stock_level, restocking_lead_time, sentiment_score, sentiment_magnitude
FROM products
EXCEPT
SELECT product_sku, product_name, total_ordered, stock_level, restocking_leadtime, sentiment_score, sentiment_magnitude
FROM sales_report;

This revealed that there were 1009 entries within the products table that were not in the sales_report table. -->


------------------------------------------------------------------------------------------------------------
SELECT product_sku, product_name, total_ordered, stock_level, restocking_leadtime, sentiment_score, sentiment_magnitude
FROM sales_report
EXCEPT 
SELECT sku, product_name, ordered_quantity, stock_level, restocking_lead_time, sentiment_score, sentiment_magnitude
FROM products;

This revealed that there were 371 entries in the sales_report table that were not in the products table.


------------------------------------------------------------------------------------------------------------
Having noted earlier that the sales_report and products tables held the same variables but contained some different values, I chose to join the like variables together using a CREATE TABLE and UNION command. Since the products table did not have a ratio variable, I needed to add one to the products table first so it could be a complete match for the sales_report table. 

ALTER TABLE products
CREATE COLUMN ratio DOUBLE PRECISION;

CREATE TABLE new_sales_report AS
SELECT product_sku, total_ordered, product_name, stock_level, restocking_leadtime, sentiment_score, sentiment_magnitude, ratio
FROM sales_report
UNION
SELECT sku, ordered_quantity, product_name, stock_level, restocking_lead_time, sentiment_score, sentiment_magnitude, ratio
FROM products;

Afterward, I removed the initial products table and the sales_report before running the same duplicate removal SELECT DISTINCT command, as previously done above. I also renamed the resulting table to product_sales_report.


------------------------------------------------------------------------------------------------------------

I subsequently ran a query to identify missing values within each table, and ensure that of the required data is present (i.e. ID numbers or identifiers that would render data nonp-usable if not present):

SELECT COUNT(*) FROM all_sessions WHERE full_visitor_id IS NULL;
SELECT COUNT(*) FROM analytics WHERE full_visitor_id IS NULL;
SELECT COUNT(*) FROM sales_by_sku WHERE product_sku IS NULL;
SELECT COUNT(*) FROM product_sales_report WHERE product_sku IS NULL;

I finally checked for outliers within the data, that are 3 standard deviations away from the average. One example query is included below, however, this would need to be completed for all integers within each table.  -->

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

If the risk of data usage is still considered high, I would recommend reaching out to a smaller group of users to run a quality control check. In the case of market research information, where it may not be possible to contact users, one could consider selecting a country and city from different regions and pulling data from the initial source to ensure consistency.

Another strategy to strengthen data and reduce risk could be to conduct a sensitivity analysis to identify errors or outliers in the data. This involves running multiple regression analyses, but with a focus on different data subsets, such as individual cities or countries, to determine if one or more locations are responsible for skewing the data and making it appear inaccurate.