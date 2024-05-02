# Sales Analysis

## Data
AdventureWorks DBs: https://learn.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-ver15&tabs=ssms 

Sales Budget (Sent Over)

## Business Request and Planning
- We need to improve our Internet Sales reports   and want to move from static reports to visual dashboards.

- Essentially, we want to focus it on how much we have sold of what products, to which clients and how it has been over time.

- Seeing as each sales person works on different products and customers it would be beneficial to be able to filter them also.

- We measure our numbers against budget so I added that in a spreadsheet so we can compare our values against performance.

- The budget is for 2024 and we usually look 2 years back in time when we do analysis of sales.


#### Making a Business Demand Overview and User Story Table to summarize and visualise all the Business requirements 

### Business Demand Overview:
-	Reporter: Steven – Sales Manager
-	Value of Change: Visual dashboards and improved Sales reporting or follow up or sales force
-	Necessary Systems: Power BI, CRM System
-	Other Relevant Info: Budgets have been delivered in Excel for 2021

### User Stories:

| No | Role               | Request                                               | User Value                                                             | Acceptance Criteria                                                        |
|----|--------------------|-------------------------------------------------------|------------------------------------------------------------------------|----------------------------------------------------------------------------|
| 1  | Sales Manager      | To get a dashboard overview of internet sales         | Can follow better which customers and products sells the best          | A Power BI dashboard which updates data once a day                         |
| 2  | Sales Representative| A detailed overview of Internet Sales per Customers   | Can follow up my customers that buys the most and who we can sell more to | A Power BI dashboard which allows me to filter data for each customer     |
| 3  | Sales Representative| A detailed overview of Internet Sales per Products    | Can follow up my Products that sells the most                          | A Power BI dashboard which allows me to filter data for each product       |
| 4  | Sales Manager      | A dashboard overview of internet sales                | Follow sales over time against budget                                  | A Power BI dashboard with graphs and KPIs comparing against budget         |

## Data Cleaning & Transformation

#### Identifying the necessary tables for doing analysis and fulfilling the business needs and extracting the following tables  using SQL.
dbo.DimCustomer             = Customer Table
            
dbo.DimProduct              =  Product Table

dbo.FactInternetSales       = Sales Table

dbo.DimDate                 = Date Table

#### Writing SQL statements for cleansing and transforming necessary data
Commenting the columns which are not required for the analysis

#### Cleansed DIM_Date Table

- Selecting required columns from [AdventureWorksDW2019].[dbo].[DimDate] and Renaming them as Date, Day, Month, MonthNo, Quarter, Year

- Adding a MonthShort Column which can be Useful for front end date navigation and front end graphs

- Using WHERE CalendarYear >= YEAR(GETDATE()) -2 to ensure we  look 2 years back in time when we do analysis of sales

#### Cleansed DIM_Customers Table

- Introducing Aliasing 

- Selecting required columns from [AdventureWorksDW2019].[dbo].[DimCustomers]and renaming them

- Adding a full name column 

- Adding CASE for M as Male and F as Female in Gender column 

- Adding customer city column from dbo.DimGeography (geography Table)

- Using LEFT JOIN on geography_key to join the geography table 

- Using ORDER BY for arranging CustomerKey in ascending order 

#### Cleansed DIM_Products Table

- Introducing Aliasing 

- Selecting required columns from [AdventureWorksDW2019].[dbo].[DimProducts] and renaming them

- Adding EnglishProductSubcategoryName column as Sub Category from dbo.DimProductSubCategory (Sub Category Table)

- Adding EnglishProductCategoryName column as Product Category from dbo.DimProductCategory (Category Table)

- Using ISNULL in Status column and renaming as outdated 

- Using LEFT JOIN on Product_Subcategory_Key to join the Sub Category table 

- Using LEFT JOIN on Product_Category_Key to join the Product table

- Using ORDER BY for arranging ProductKey in ascending order

#### Cleansed DIM_Fact_InternetSales Table

- Selecting required columns from [AdventureWorksDW2019].[dbo].[FactInternetSales]

- Using WHERE LEFT (OrderDateKey, 4) >= YEAR(GETDATE()) -2 to Ensure we always only bring two years of date from extraction

- Using ORDER BY for arranging OrderDateKey in ascending order 

Exporting Data as CSV from all queries 


## Dashboard 
- Loading the extrated data from SQL and the sent over Sales Budget and transforming the data in PowerBI and categorising Tables into fact and Dimension  

   Fact tables contain numerical data, while dimension tables provide context and background information.

- Data Modelling - defining relationships between tables, creating hierarchies, and optimizing data for efficient querying and visualization                                                      

    Connecting Date , CustomerKey and ProductKey from Dimension Tables to Fact Tables

- Changing Data Category of Customer city from uncategorized to city so that we can use a globus later  

#### Calculating Measures

 Sales = SUM( FACT_InternetSales_SQLData[SalesAmount] )

 Budget Amount = SUM(FACT_Budget[Budget])

 Sales / Budget Amount = DIVIDE( FACT_InternetSales_SQLData[Sales] ,FACT_Budget[Budget Amount]) in percentage 

 
 #### Dashboard Designing 

 Page 1 - Sales Overview

 Page 2 - Customer Overview

 Page 3 - Product Overview



