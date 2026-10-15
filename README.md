Power BI Assignment 1 – Data Transformation & Data Modeling
Project Overview
This project focuses on preparing and modeling an E-commerce Sales dataset in Microsoft Power BI. The assignment covers importing data, cleaning and transforming fields in Power Query, combining related tables, aggregating business metrics, and designing relationships for reliable analysis.
Objectives
•	Import and prepare multiple sales-related datasets.
•	Standardize data types and text formatting.
•	Create useful calculated and conditional columns.
•	Identify and address missing and duplicate records.
•	Merge, sort, filter, and aggregate sales data.
•	Build and validate relationships between tables.
Tools and Technologies
•	Microsoft Power BI Desktop
•	Power Query Editor (Power Query M)
•	CSV data sources
•	Power BI data modeling and Manage Relationships
Datasets
Dataset	Fields	Purpose
List of Orders	Order ID, Order Date, Customer Name, City, State, Category, Amount, Profit	Order and customer information
Order Details	Order ID, Category, Sub-Category, Quantity, Amount, Profit	Product-category-level sales details
Sales Target	Category, Month of Order Date, Target	Monthly category sales targets
Note: The dataset fields above reflect the assignment brief. Confirm actual column names and table grain in Power Query before applying steps.
Data Preparation and Transformation
1. Data Import
Import List of Orders.csv, Order Details.csv, and Sales Target.csv using Home → Get Data → Text/CSV, then select Transform Data to open Power Query Editor.
2. Row Limiting and Data Types
•	Keep the first 500 rows of List of Orders (Home → Keep Rows → Keep Top Rows).
•	Set Order Date to Date.
•	Set Amount and Target to Fixed decimal number (Currency in Power Query), where appropriate.
•	Check that Profit and Quantity have suitable numeric types.
3. Text Formatting
Convert Customer Name (or CustomerName, depending on the source) to Capitalize Each Word for consistent capitalization.
4. Custom Columns
Create Location in the format City, State:
Text.Combine(List.RemoveNulls({[City], [State]}), ", ")
Create Profit Margin:
if [Amount] = null or [Amount] = 0 then null else [Profit] / [Amount]
Set the new column's data type to Percentage. Do not multiply by 100 if using Percentage formatting.
5. Conditional Column
Create Profit Status based on the profit value:
if [Profit] = null then "Unknown"
else if [Profit] < 0 then "Loss"
else if [Profit] = 0 then "Break-Even"
else "Profit"
6. Merge Tables
Merge List of Orders and Order Details on Order ID, using a Left Outer join to retain all orders from the limited List of Orders table. Expand the required Order Details columns and name the resulting query Orders Data.
Important: An order can have multiple detail rows, so the merged result may contain more than 500 rows. Avoid double-counting order-level amounts or profits after expansion.
7. Missing Values and Duplicates
•	Inspect nulls, blanks, errors, and column quality in all three datasets.
•	Investigate missing Order ID values before joining; remove unusable key records only when justified.
•	Preserve meaningful missing values instead of automatically replacing them with zero.
•	Remove exact duplicate records when confirmed to be accidental.
•	Do not remove repeated Order ID values from Order Details when they represent legitimate line items.
•	Document every cleaning decision.
8. Sorting and Filtering
•	Sort Orders Data by Order Date in descending order.
•	Filter State to a selected region such as Tamil Nadu for focused analysis.
•	Keep a full-data query if the state filter is intended only for exploration, rather than restricting the entire final model.
9. Grouping and Aggregation
Duplicate or reference Order Details to prepare summaries:
Summary	Group by	Aggregation
Order count	As required by the analysis	Count of Order ID (rows, or distinct count if specified)
Average profit	Category	Average of Profit
Total sales amount	Sub-Category	Sum of Amount
Duplicate or reference Sales Target and group by Month of Order Date to calculate the Sum of Target. Include year when the data spans multiple years to avoid combining different years' months.
10. Data Modeling
The assignment requests relationships between:
1.	List of Orders and Order Details on Order ID.
2.	Order Details and Sales Target on Category.
Use Model view → Manage Relationships to review cardinality, filter direction, and active status.
Modeling caution: Order Details commonly contains multiple rows per order, and Sales Target commonly contains multiple rows per category (one per month). A direct relationship on Category may therefore be many-to-many and can cause ambiguous totals. A more reliable analytical model uses distinct Orders, Category, and Date/Month dimension tables with one-to-many relationships to the appropriate fact tables. Verify that order IDs are unique on the Orders side before defining a one-to-many relationship.
Key Business Metrics
•	Total Sales: Sum of sales Amount at the appropriate grain.
•	Total Profit: Sum of Profit at the appropriate grain.
•	Profit Margin: Total Profit ÷ Total Sales.
•	Order Count: Distinct count of Order ID.
•	Target Achievement: Actual Sales ÷ Sales Target, after aligning category and month.

