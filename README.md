Customer Shopping Behavior Analysis
An end-to-end data analytics project exploring customer purchasing patterns with Python, Pandas, PostgreSQL, SQL, and Power BI. The project uses 3,900 shopping records to study revenue, product preferences, discount behavior, shipping, customer segments, and subscription adoption.
Project Overview
Businesses need to understand what customers buy, how much they spend, how discounts affect purchases, and whether subscription and loyalty strategies are reaching their goals. This project prepares and analyzes customer shopping data, uses SQL to answer business questions, and presents the findings in a Power BI dashboard.

Project Objectives
- Analyze customer spending and revenue by demographic groups.
- Identify highly rated and frequently purchased products.
- Compare Standard and Express shipping purchase amounts.
- Examine discount usage and subscription adoption.
- Segment customers into New, Returning, and Loyal groups.
- Present key findings through a Power BI dashboard.
- Recommend practical actions to improve customer retention and marketing.
  
Dataset
The provided CSV contains 3,900 rows and 18 columns.
Data area	Fields
Customer profile	Customer ID, Age, Gender, Location
Purchase details	Item Purchased, Category, Purchase Amount (USD), Season, Size, Color
Customer behavior	Subscription Status, Shipping Type, Discount Applied, Promo Code Used, Previous Purchases, Payment Method, Frequency of Purchases
Feedback	Review Rating


Data quality: The dataset has 37 missing values in Review Rating. The notebook fills missing ratings using the median review rating within the corresponding product category.

Tools and Technologies
- Python — data preparation and analysis
- Pandas — data cleaning, transformation, and feature engineering
- Jupyter Notebook — interactive analysis workflow
- PostgreSQL — storage of cleaned data
- SQL — business-focused queries and aggregations
- SQLAlchemy / psycopg2 — Python-to-PostgreSQL connection
- Power BI — interactive reporting and visualization
  
Project Workflow
1. Load and explore the data
The notebook reads customer_shopping_behavior.csv with Pandas and explores the dataset using methods such as head(), info(), describe(), and null-value checks.
2. Clean missing review ratings
Missing values in the Review Rating column are imputed using the median rating for the relevant product category.
3. Standardize column names
Column names are converted to lowercase snake case. The purchase amount field is renamed to purchase_amount_usd for easier use in Python and SQL.
4. Engineer features
The notebook adds:
- age_group — groups ages into four quantile-based groups labelled Young Adult, Adult, Middle Aged, and Senior.
- purchase_frequency_days — maps purchase-frequency labels to approximate day intervals.
5. Review overlapping fields
The notebook compares discount_applied and promo_code_used and drops promo_code_used. If you extend the project, validate that these fields are truly redundant before removing one, because using a discount and using a promo code may not always mean the same thing.
6. Load data into PostgreSQL
The cleaned DataFrame is loaded into a PostgreSQL table named customer in the customer_behavior database using SQLAlchemy.
7. Perform SQL analysis
The SQL script investigates ten business questions, described below.
8. Visualize insights in Power BI
The Power BI file customer_behaviour_dashboard.pbix presents the analysis as a dashboard. The accompanying presentation summarizes selected findings and business actions.

SQL Business Questions
The included customer_behaviour_queries.sql contains queries to explore:
1. Revenue by gender — compare total purchase revenue for male and female customers.
2. High-spending discount users — identify discount users whose purchase amount is at least the overall average.
3. Top five products by review rating — rank products by average review rating.
4. Shipping comparison — compare average purchase amounts for Standard and Express shipping.
5. Subscribers vs. non-subscribers — compare record counts, average spend, and total revenue by subscription status.
6. Products with high discount rates — find five products with the highest percentage of purchases marked as discounted.
7. Customer segmentation — count New, Returning, and Loyal records based on previous_purchases.
8. Top three products per category — rank purchased products within each category.
9. Repeat buyers and subscriptions — compare subscription status among records with more than five previous purchases.
10. Revenue by age group — calculate total revenue for each engineered age group.
Selected Findings

The accompanying project presentation reports these findings:
Metric / finding	Reported result
Purchases analyzed	3,900
Fields in the source dataset	18
Average purchase amount	$59.76
Average review rating	3.75
Review ratings imputed during cleaning	37
Male customer revenue	$157,890
Female customer revenue	$75,191
Discount users spending above the average purchase amount	839
More non-subscribers than subscribers	1,794
Repeat-buyer records with subscription	958
Repeat-buyer records without subscription	2,518
Express shipping average purchase amount	$60.48
Standard shipping average purchase amount	$58.46

The presentation also highlights Clothing as the leading category in the dashboard's revenue and sales views. It identifies Gloves (3.86), Sandals (3.84), Boots (3.82), Hat (3.80), and Skirt (3.78) among the top-rated products.

Interpretation: The presentation frames subscription adoption as a retention opportunity. It recommends testing subscription benefits with repeat buyers, building loyalty, targeting discounts, promoting high-rated products, and tailoring campaigns to higher-revenue customer groups.
Figures above are transcribed from the accompanying presentation. Check that the Power BI dashboard and SQL outputs use the same definitions and filters before presenting them as final business KPIs.

Repository Structure
Customer-Shopping-Behavior-Analysis/
├── customer_shopping_behavior.csv
├── Customer_Shopping_Behavior_Analysis.ipynb
├── customer_behaviour_queries.sql
├── customer_behaviour_dashboard.pbix
├── Customer-shopping-behavior-points-to-a-retention-opportunity.pptx
└── README.md
This structure reflects the files provided for this project. You may add an images/ folder for dashboard screenshots and a requirements.txt file for Python dependencies.

How to Run the Project

Prerequisites
- Python 3.10 or a compatible Python version
- Jupyter Notebook or VS Code with notebook support
- PostgreSQL
- Power BI Desktop to open the .pbix file
  
1. Install Python dependencies
pip install pandas sqlalchemy psycopg2-binary jupyter
2. Open the notebook
Open Customer_Shopping_Behavior_Analysis.ipynb in Jupyter Notebook or VS Code. Keep the CSV in the same folder as the notebook, or update the file path in the first data-loading cell.
3. Configure PostgreSQL
Create the database used by your local setup:
CREATE DATABASE customer_behavior;
Update the connection configuration in the notebook to match your local PostgreSQL username, password, host, port, and database. Do not publish database credentials in a public repository. Prefer environment variables or a local configuration file excluded from Git.
Run the notebook cells in order to clean the data and load it into the customer table.
4. Run the SQL queries
Open customer_behaviour_queries.sql in pgAdmin, DBeaver, or another PostgreSQL client connected to the database and execute the queries.
5. Open the Power BI dashboard
Open customer_behaviour_dashboard.pbix in Power BI Desktop. If prompted, update the data source connection and refresh the data.
Important Validation Notes
Review these points before reproducing the project from a clean setup:
- The notebook maps the label Fornightly, while the dataset contains Fortnightly. Correct the spelling in the mapping so that fortnightly records are mapped to 14 days.
- The final age-group SQL query uses purchase_amount, while the notebook standardizes the field as purchase_amount_usd. Make the column name consistent before executing that query.
- The SQL script segments records based on previous_purchases; because the dataset has 3,900 purchase records, the resulting counts may represent rows/records rather than distinct real-world customers. Describe them accordingly unless the analysis is changed to aggregate by customer.
- The customer-segmentation SQL labels previous_purchases = 1 as New, values 2–10 as Returning, and all other values as Loyal. Document that rule when explaining the results.
- Validate the discount_applied / promo_code_used comparison before treating the fields as interchangeable.
- The notebook currently includes example PostgreSQL credentials in its connection cell. Replace them with your own local configuration and never commit real credentials.
- The notebook uses if_exists='replace' when writing the table, which replaces the destination table each time that cell is run.
Business Recommendations
1. Boost subscriptions: Communicate subscriber benefits clearly and test offers aimed at repeat buyers.
2. Build loyalty: Reward returning customers and encourage progression toward the Loyal segment.
3. Review discount policy: Use targeted promotions and monitor profitability or margin impact.
4. Promote strong products: Feature top-rated products and category leaders in campaigns.
5. Target higher-value cohorts: Tailor marketing by age group, gender, and shipping preferences, and measure campaign performance.
   
Skills Demonstrated
- Data cleaning and quality checks
- Exploratory data analysis (EDA)
- Feature engineering with Pandas
- PostgreSQL database integration
- SQL aggregations, subqueries, CTEs, and window functions
- Customer and product analysis
- Power BI reporting
- Translating data findings into business recommendations
  
Future Improvements
- Add a dashboard screenshot and label the main KPIs.
- Add query outputs or result tables for each business question.
- Correct the frequency mapping typo and SQL column-name mismatch.
- Validate customer-level versus transaction-level definitions.
- Add automated data validation and reproducible setup instructions.
- Add profit/margin data to evaluate discount effectiveness if available.
- Add customer retention or cohort analysis if suitable date and customer identifiers are available.
  
Author
PANDIYAN. M
- GitHub: https://github.com/PANDIYAN04
- LinkedIn: www.linkedin.com/in/pandiyan-m04
