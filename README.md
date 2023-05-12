# Final-Project-Transforming-and-Analyzing-Dataupdate-with-SQL

## Project/Goals
* Set up, clean, and analyze the e-commerce database via SQL
*  Understand different connections across datasets
*  Pull information to answer EDA questions, as well as create queries to explore the data further. 
* Conduct a QA walkthrough of the database to identify and mitigate risks within the data and think through the entire process of data cleaning, transformation, and analysis.

## Process

At the beginning of the project, I created five tables and uploaded data from CSV documents. After this, I moved on to the data cleaning process, where I searched for a range of potential issues, including outliers, missing and incomplete data, duplicated entries, incorrect data types, and inconsistent formatting or naming conventions.

To identify outliers, I used statistical variance queries up to three degrees and imputed null values for any missing values. This helped me identify two outliers under the visit ID variable in the analytics table and 13,308 outliers under the unit price variable, which I noted down for reference.
 
Although the analysis did not have specific goals, I conducted a primary check for outliers before removing duplicates. I kept in mind that it might be necessary to re-check for outliers after removing duplicates since the statistical properties of the dataset can change.
 
During an initial review of the database, I noticed that the "sales_report" and "product" dataset variables appeared to be duplicated. To investigate, I executed a JOIN query between those two tables, which revealed that all variable names, except for an extra ratio column in the sales table, were a match. I then conducted an EXCEPT query to review duplication across the tables, revealing that there were 1009 entries in the "products" table that were not in the "sales_report" table, and 371 entries in the "sales_report" table that were not in the "products" table. I made a note to join these tables once further cleaning had been conducted.
 
The next step in the cleaning process was to remove non-significant variables with empty rows that could not be calculated using available information. Variables such as "total_transaction_revenue", "transactions", "session_quality_dim", "product_refund_amount", "product_quantity", "product_revenue", "item_revenue", "item_quantity", "transaction_revenue", "transaction_id", "search_keyword", and "ecommerce_action_option" from the "all_sessions" table appeared to return NULL values for all of their rows. Similarly, "units_sold", "revenue", "user_id", and "time_on_site" variables from the "analytics" table also had more than 95% NULL values, except for the "time_on_site" variable. Hence, I used a COUNT DISTINCT query to check for unique results for each variable. For the five variables from the "all_sessions" table that showed all NULL values, I dropped them using a DROP COLUMN command. For all the other variables that had more than one distinct value (NULL), I ran a COUNT IF NULL query to see how many of the total values were NULL. If the amount of NULL was greater than 70% then I removed them using a DROP COLUMN command.

The following step in the cleaning process was to remove all duplicates across each dataset. I removed duplicates within tables using three commands: CREATE TABLE with SELECT DISTINCT, DROP TABLE, and ALTER TABLE. I also reviewed and removed duplicates across rows. Since the sales_report and products tables had the same variables but contained different values, I joined the similar variables using a CREATE TABLE and UNION command. However, before doing so, I added a ratio variable to the products table to ensure a complete match with the sales_report table. Afterward, I dropped the original products table and sales_report table and renamed the resulting table as product_sales_report.

Subsequently, I addressed any incorrect data types present in the dataset. Upon reviewing the analytics table and the all_sessions table, I discovered that the unit_price column and the product_price column respectively contained values that appeared to be in millions but actually represented a dollar value. To rectify this, I divided the values in these columns by 1,000,000.

Finally, I standardized the formatting of all variable titles to adhere to the word_word format. If there were similar variables formatted differently under different tables, I corrected them. All variable titles and table names were kept in lower case to ensure consistency throughout the entire dataset.

With additional time for data cleaning, I would proceed by further joining the sales_by_sku table with the products_sales_reports table to consolidate the duplicate variables present in both tables. Additionally, I would recommend restructuring the collected data into more meaningful categories, such as financial information, customer data and information, and product stock and warehouse information, to enhance the overall organization of the database.

Moreover, I would address further formatting and duplicate issues, such as standardizing the format of the Date variable under all_sessions to only include year/month/day without the time component. I would also convert the large full_visitor_id numbers from integers to character strings to ensure consistency in data type across the database. Furthermore, I would thoroughly review row values to eliminate duplicates among individual table values that may suggest an error in the overall database.


## Results
The data from the Ecommerce store, does not look promising. Below are some highlights from data reviewed during analysis:

- The ecommerce website received its highest traffic of 1,833 visitors during its first month of launch in August 2016, while traffic was at its lowest with only 37 visitors in August 2017. This trend indicates a decreasing popularity of the website over time, suggesting that the marketing methods need to be re-evaluated and adjusted accordingly to increase website traffic. 

- A majority of visitors accessed the ecommerce website through organic searches, indicating the importance of optimizing the website for search engines. 

- Out of the 8,653 visitors who accessed the website via organic searches, they collectively viewed 36,249 pages, which averages out to around 4 pages per visitor. This suggests that visitors who actively sought out the website, were also engaged with the content on the website as they were viewing multiple pages.

- Affiliates and display access drove the lowest amount of website users to access the ecommerce website. This implies that these marketing methods were ineffective in driving traffic to the website, and given the associated costs, it is recommended to either discontinue these services or seek new affiliate marketers or displays.

-  22.5% of visitors who accessed the website through an organic search returned to the ecommerce website at a later date. This highlights the importance of providing a positive user experience to encourage repeat visits. To determine the factors that encourage visitors to return to the website,  further qualitative data or survey research could be conducted to provide insight into what aspects of the website visitors find appealing. 

- Several products in the ecommerce store, including the Cam Outdoor Security Camera - USA, Learning Thermostat 3rd Gen-USA in Stainless Steel and Copper finishes, the Waterproof Backpack, the Men's Watershed Full Zip Hoodie in Grey, the Cam Indoor Security Camera - USA, the Learning Thermostat 3rd Gen-USA in White finish, and a $250.00 Gift Card, generated profits of less than 0.01%. This indicates that the unit cost of these products may be too high, or the selling price may be too low to generate a significant profit margin. To improve profitability, the ecommerce store may need to consider finding a new manufacturer with lower unit costs or exploring alternative sourcing methods. Alternatively, increasing the selling price of these products slightly could also help to increase the profit margin without significantly impacting sales volume. However, it's important to keep in mind that increasing prices too much could also negatively impact sales and customer satisfaction. Therefore, a careful balance between pricing and profitability needs to be maintained.

- The purchasing trends across different regions and cities are diverse and offer valuable insights into the needs and preferences of customers. From stationary and school supplies in Asia-Pacific to hygiene and personal care products in South America, it is evident that different products appeal to different regions. While some regions show clear favorites in terms of top purchases, others demonstrate a more varied buying pattern.

- A significant proportion of purchases did not have a listed city or country associated with them, indicating a gap in the dataset. Addressing this gap in the future will provide a more comprehensive understanding of customer preferences and help tailor the product offerings to meet their needs better.

- To optimize sales and improve customer satisfaction, it would be wise to refine the product selection based on popular regional preferences. By gradually scaling the business and expanding the product line based on consumer demand, the store could be made more manageable while keeping costs low. Strategic branding and targeted marketing of products to certain regions, including developing culturally relevant products, would also support future growth.

- Despite the promising growth of the ecommerce store and an initial increase in revenue since its opening, a closer look at the data reveals a decline in revenue outside of the early summer months, specifically May to July. The most significant drop-off in revenue occurred in August 2017, the last month for which data was collected. It would be helpful to know the data cut-off date to determine if this is an accurate representation of the entire month. If it is, this suggests a significant issue that requires attention. 

After reviewing the collected data from the ecommerce store, it is evident that it is not reliable and consistent, which makes it an untrustworthy source for generating meaningful insights. This implies that any market research based on this data should be considered high-risk as it may lead to incorrect conclusions. The limitations of the dataset may also impact the accuracy of the conclusions drawn from it.

To reduce the risks that come with relying on potentially inaccurate or incomplete information, conducting thorough market research using various sources of data is highly recommended. Additionally, restructuring the data into more meaningful categories, such as financial information, customer data and information, and product stock and warehouse information, can significantly improve the overall organization of the database. By implementing these measures, the ecommerce store can make well-informed decisions and develop effective strategies to enhance its performance and drive growth.

## Challenges 

One challenge was that many of the variables across the datasets had similar names.  While some were the same variable and needed to be cleaned, others such as product_price and unit_price were confusing.  For certain questions, I had to make assumptions and then justify my use of a variable that made the most sense to generate a query. One example would be the inclusion of both product_price and unit_price in the products and analytics datasets. I determined that product_price would be the direct amount a customer would pay and would account for other costs such as discounts or shipping, whereas the unit price from analytics would be just the base price of the physical product without anything else considered. Therefore, I used product price to generate revenue. However, it is possible that without accurate information or a descripton of each variable,  even correct queries could retrieve incorrect data.


## Future Goals
With more time I would have conducted a more throrough cleaning and analysis of the database. Although, I was able to remove exact duplicates from the table, there is still other data that could have been sythesized. 
