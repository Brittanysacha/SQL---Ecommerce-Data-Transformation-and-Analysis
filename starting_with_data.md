Question 1: After taking into account the unit cost, what portion of the product price represents the profit percentage?

SQL Queries:

SELECT products_sales_report.product_name, ((all_sessions.product_price / analytics.unit_price) * 100) AS profit_percentage
FROM products_sales_report, all_sessions, analytics
WHERE products_sales_report.product_sku = all_sessions.product_sku 
AND all_sessions.full_visitor_id = analytics.full_visitor_id
AND all_sessions.product_price != 0
AND analytics.unit_price != 0
ORDER BY profit_percentage DESC;

Answer: 
The products  Cam Outdoor Security Camera - USA,  Learning Thermostat 3rd Gen-USA - Stainless Steel, Waterproof Backpack,  Learning Thermostat 3rd Gen-USA - Copper,  Men's Watershed Full Zip Hoodie Grey,  Cam Indoor Security Camera - USA,  Learning Thermostat 3rd Gen-USA - White, Gift Card - $250.00 all generated less than a 0.01% profit. I would recomend either finding a lower product manufacturer to lower the unit cost or charging a slightly higher price for the product. 


Question 2: How did visitors access the ecommerce website? 

SQL Queries: 

SELECT
  channel_grouping AS access_method,
  COUNT(*) AS number_of_clicks,
  SUM(page_views) AS total_page_views,
  COUNT(DISTINCT full_visitor_id) AS unique_visitors
FROM
  all_sessions
GROUP BY
  channel_grouping;

Answer: 

"access_method"	"number_of_clicks"	"total_page_views"	"unique_visitors"
"(Other)"	        5	                    14	                5
"Affiliates"	    265	                    1192	            245
"Direct"	        2997	                13226	            2805
"Display"	        125	                    600	                119
"Organic Search"	8653	                36249	            8181
"Paid Search"	    509	                    2984	            470
"Referral"	        2580	                14214	            2417

The ecommerce website was mostly accessed through organic searches by the majority of its visitors. Of the 8653 visitors who accessed the website, they collectively viewed 36,249 pages, averaging around 4 pages per visitor. Affiliates and display access drove the lowest amount of website users to access the ecommerce websitt. This indicates that affiliates and displays were ineffective in driving traffic to the website. Given the associated costs, it is recommended to either discontinue these services or seek new affiliate marketers or displays.

Additionally, 22.5% of the visitors who accessed the website through an organic search returned to the ecommerce website at a later date. To determine the factors that encourage visitors to return to the website, further qualitative data or survey research should be considered.


Question 3: How long on average in minutes did people from different countries spend on the eccomerce website?

SQL Queries:

SELECT all_sessions.country, AVG(all_sessions.time_on_site) AS avg_time_on_site
FROM all_sessions 
GROUP BY all_sessions.country
ORDER BY avg_time_on_site DESC;

Answer: 

The ecommerce website did not receive any traffic from users in Ethiopia, Sint Maarten, Montenegro, Somalia, Jordan, Haiti, Zimbabwe, Rwanda, Lebanon, Tanzania, Botswana, or Algeria. To ensure that the website is active and accessible in these regions, it should be checked for any technical issues. If the website is functioning properly, the data from these regions should be examined to identify any reasons why it is not being tracked or recorded.

The ecommerce website had the longest average session duration among visitors from Jersey, Trinidad & Tobago, El Salvador, Kenya, Slovenia, Tunisia, Nigeria, and Peru. These countries merit further investigation to identify any regional factors that may contribute to the longer session durations on the website.



Question 4: How much traffic did the ecommerce website revieve each month? 

SQL Queries: 

SELECT DATE_TRUNC('month', date) AS month, COUNT(DISTINCT full_visitor_id) AS visitors
FROM all_sessions
GROUP BY month
ORDER BY month;

Answer:

The ecommerce website received its highest traffic of 1,833 visitors during its first month of launch in August 2016, while traffic was at its lowest with only 37 visitors in August 2017. This trend indicates a decreasing popularity of the website over time, suggesting that the marketing methods need to be re-evaluated and adjusted accordingly to increase website traffic.  

DATE        TRAFFIC 
2016-08     1833
2016-09     1471
2016-10     966
2016-11     1081
2016-12     1180
2017-01     1023
2017-02     1048
2017-03     1058
2017-04     1061
2017-05     1208
2017-06     1137
2017-07     1250
2017-08     37





Question 5: How much revenue has the commerce store generated/per month since opening?

SQL Queries:

Answer:
