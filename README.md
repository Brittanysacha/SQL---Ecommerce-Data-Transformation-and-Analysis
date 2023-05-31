# E-commerce Database Transformation and Analysis

## Project/Goals

- Set up, clean, and analyse the e-commerce database using SQL.
- Understand different connections across datasets.
- Pull information to answer exploratory data analysis (EDA) questions and create queries to further explore the data.
- Conduct a QA walkthrough of the database to identify and mitigate risks within the data and consider the entire process of data cleaning, transformation, and analysis.

## Process

During the project, I undertook the following steps:

1. Created five tables and uploaded data from CSV documents.
2. Carried out data cleaning, addressing various potential issues such as outliers, missing and incomplete data, duplicated entries, incorrect data types, and inconsistent formatting or naming conventions.

   - Used statistical variance queries to identify outliers and imputed null values for missing data.
   - Checked for outliers again after removing duplicates, recognising that the statistical properties of the dataset can change.
   - Noticed duplication between the "sales_report" and "product" dataset variables. Executed a JOIN query between these tables and used an EXCEPT query to review duplication across tables.
   - Removed non-significant variables with empty rows that couldn't be calculated using available information.
   - Dropped variables that had more than 70% NULL values and retained variables with distinct values.
   - Removed duplicates within tables using various commands.
   - Joined similar variables from the "sales_report" and "products" tables, dropped the original tables, and renamed the resulting table.
   - Rectified incorrect data types by dividing values in certain columns by 1,000,000.
   - Standardised the formatting of variable titles to adhere to the word_word format and maintained lowercase consistency.

3. Identified further steps for data cleaning and enhancement:

   - Join the "sales_by_sku" table with the "product_sales_report" table to consolidate duplicate variables.
   - Restructure the collected data into meaningful categories, such as financial information, customer data, and product stock and warehouse information, to enhance the overall organisation of the database.
   - Address formatting and duplicate issues, such as standardising the format of the "Date" variable and converting large "full_visitor_id" numbers to character strings.
   - Conduct a thorough review of row values to eliminate duplicates among individual table values that may suggest an error in the overall database.

These additional steps would further improve the data cleaning process and organisation of the e-commerce database, enhancing its quality and enabling more comprehensive analysis.

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
