# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
There were several goals within this project related to learning how to set up, clean, and analyze a database via SQL, while also transferring information from source code to GitHub. The project further required learning how to understand different connections across datasets, pull information to answer questions, as well as create queries to explore the data further. Once the data was cleaned and analyzed, we conducted a QA walkthrough of the database to identify and mitigate risks within the data and think through the entire process of data cleaning, transformation, and analysis.

My personal goals for this project were to develop a better working understanding of how to use SQL and pgAdmin. However, I also feel that I gained valuable experience in troubleshooting when encountering issues with query writing. Specifically, I improved my ability to identify errors and debug my written code. I would hope this is transferable into learning other coding languages and applications such as Python, mySQL, or Java.

## Process
The first step of my project was to assemble the databases, creating five tables and importing data from individual CSV files. Before adding any data, I examined my ERD to identify possible primary keys and connections between tables. Once all CSV files were loaded, I began the data cleaning process. 

For the data cleaning process, I first outlined the steps I needed to complete and then searched for additional query methods and commands on Stack Overflow and Postgresql Tutorial to complete steps I had not learned how to do, with some assistance from mentors along the way. The most important step was to remove all variables with only NULL values, as well as duplicate rows and columns. This reduced the dataset size and made it easier to work with, allowing me to synthesize my ERD table to better assess connections and answer questions in the starting_with_data section of the assignment.

My data cleaning process was as follows: identify outliers, impute missing or incomplete data,  remove duplicate entries, fix incorrect data types, and finally correct inconsistent formatting and naming conventions. 

For outliers, To identify outliers, I executed statistical variance queries up to three degrees. I further imputed NULL for missing values. It is important to note that although I checked for outliers prior to removing duplicates, both can cause their own set of issues across a database. Unidentified outliers can skew analysis results, leading to incorrect conclusions, while duplicates can create bias within analysis results, and also lead to correct conclusions. The step you take first should be determined by the overall goals of the analysis. Although the questions under the starting_with_questions section would suggest the analysis seeks market information, I personally did not have specific goals for the analysis. Therefore, I conducted a primary check for outliers before removing duplicates, with the understanding that after removing duplicates, it may be necessary to re-check for outliers. This is because the statistical properties of the dataset after duplictae removal can change.

During the search for outliers I noticed a significant amount of dupicates 


For the star


## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)

## Challenges 
I faced a few challenges during this project. Firstly, while I had a general idea of how I wanted to clean the database and what queries I wanted to run, I struggled with executing them correctly using SQL and pgAdmin. I had a fair amount of trial and error trying to format using correct sytax and proper ordering, and find the correct commands. Stack Overflow and Postgresql Tutorial, alongside other information from the weeks lessons was very helpful. 

Another challenge was that many of the variables across the datasets had similar names.  While some were the same variable and needed to be cleaned, others such as product_price and unit_price were confusing.  For certain questions, I had to make assumptions and then justify my use of a variable that made the most sense to generate a query. One example would be the inclusion of both product_price and unit_price in the products and analytics datasets. I determined that product_price would be the direct amount a customer would pay and would account for other costs such as discounts or shipping, whereas the unit price from analytics would be just the base price of the physical product without anything else considered. Therefore, I used product price to generate revenue. However, it is possible that without accurate information or a descripton of each variable,  even correct queries could retrieve incorrect data.

Finally, I had to prioritize completing each task to a sufficient level, and therefore I did not have enough time to conduct as in-depth data cleaning and analysis as I would have liked. Although I was not able to clean the database as thoroughly as I would have preferred, I documented the data cleaning steps that I had completed and identified what additional work I would have liked to do. If this had been a team project or a professional work environment where I had been reassigned, I would have hoped that my documentation provided enough detail for someone else to pick up where I left off.


## Future Goals
With more time I would have conducted a more throrough cleaning and analysis of the database. Although, I was able to remove exact duplicates from the table, there is still other data that could have been sythesized. 