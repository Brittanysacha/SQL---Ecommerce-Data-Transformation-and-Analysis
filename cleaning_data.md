<!-- What issues will you address by cleaning the data?
When cleaning the dataset, I wanted to check for a range of potential data issues including:

-	Missing or incomplete data
-	Duplicate entries
-	Incorrect data types
-	Outliers or extreme values
-	Inconsistent formatting or naming conventions -->

<!-- To begin to address these issues, I wanted to perform a range of data cleaning tasks, including: -->

<!-- I would start by reviewing outliers, imputing NULL for missing values, and then running the following code for each integer across my tables to identify rows that are more than 3 standard deviations away from the mean. --> -->

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

<!-- No outliers returned for all_sessions - time_on_site -->

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

<!-- No outliers returned for all_sessions - time_on_site -->

<!-- At this time, data would be either omitted or highlighterd based on the parameters of what was being asked of from a business or organization. For instance, they may decide that the outliers warrant an investigation, or missing values warrant further outreach for clarification. -->


-	Removing rows or columns with missing or incomplete data.
-	Removing duplicate entries.
-	Converting data types to the appropriate formats (i.e. converting certain numbers to strings – such as phone numbers ).
-	Standardizing formatting or naming conventions (i.e. using M.





Queries:
Below, provide the SQL queries you used to clean your data.
