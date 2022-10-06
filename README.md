![BA](https://user-images.githubusercontent.com/114446174/193911846-190b0e80-3313-48e6-aee9-7a4e5ffe2f70.jpeg)

# Power-BI-Product-Rationalization

## **Business Context**
An American bike company called Pro Bikes Inc. has two primary sales channels. These are the physical stores and online stores. Within the online channel, the company sells bikes from its own website and from other popular online platforms. The company sells various bikes and bike accessories such as helmets, tires, knee pads, water bottles, etc.  

## **Problem statement**
The company has three brands of bikes and each brand has multiple sizes, colors and related varieties. In total, the company sells 125 unique bikes. Managing such a high number of SKUs (stock keeping units) is associated with cost in form of inventory technology. The management wants to conduct a product rationalization exercise to fine tune its product portfolio. It has assigned the responsibility to its lead business analyst.

## **Business Analysis**
As a business analyst, I’ve defined the scope of the exercise. It includes creating a mechanism to identify products that make financial sense for the company and identify the ones that do not. I’ve made three buckets in which products need to be categorized. The purpose of 'bucketing' products is to make data driven recommendations on whether or not to retire products that are not contributing, so that company’s resources can be utilized on other products efficiently. The buckets are as follows.

1.	Top contributor – Products with high sales and high margin
2.	Bottom contributor – Products with low sales and low margin
3.	Middle contributor – Products with decent sales and decent margin


## **Requirement Gathering (User stories)**
The three buckets form part of the requirements. Following are the requirements captured in user story format.

![image](https://user-images.githubusercontent.com/114446174/193912846-391c437a-d6f4-4ba4-ac25-bb49128063ea.png)

## **Data availability**
Since customer information is sensitive, the data resides in company’s secured SQL server. I have been granted a read only access to the SQL database to perform analysis. The sales information is available in the fact table named “dbo.FactInternetSales”. In addition to SQL database, I am going to use an excel sheet that contains mapping of products to product categories

# **Pre-requisites**
Apart from the pre-installed Microsoft Office Suite, we need to install 2 important softwares.

1. **Power BI Desktop** is used which is an open source Data Visualization software created by Microsoft as part of the Microsoft Business Intelligence Toolkit. <br />
1.1 Download Power BI from [here](https://powerbi.microsoft.com/en-us/downloads/).<br />
1.2 Step by step installation guide is [available](https://www.youtube.com/watch?v=T_qnV-HTb-M).

2. **Microsft SQL Server**, a relational database management system developed by Microsoft is used for Data Storage, Retrieval and data transformation.<br />
2.1 Download the SQL Server 2019 [Developer version](https://www.microsoft.com/en-in/sql-server/sql-server-downloads)<br />
2.2 SQL Server 2019 Developer Step by step [installation guide](https://www.youtube.com/watch?v=7GVFYt6_ZFM).

3. **Microsoft SQL Server Management Studio**, a software application first launched with Microsoft SQL<br />
3.1 [Download](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15) SQL Server Management Studio 18.10<br />
3.2 [SQL Server Management Studio](https://www.youtube.com/watch?v=CqpURYqK_wU) - Step by step installation guide.

# **Data Extraction, Transformation and Loading**

## **1. Create query to extract data from SQL database** <br />

```
SELECT 
      [CustomerKey]
      ,[OrderQuantity]
      ,[UnitPrice]
      ,[DiscountAmount]
      ,[CustomerPONumber]
      ,[OrderDate]
  FROM [AdventureWorksDW2019].[dbo].[FactInternetSales]
```
## **2. Import data in Power BI and apply transformations in Power Query Editor** <br />

***2.1 Data type change*** : It can be seen that the data type of “OrderDate” column in Customer table is Date\Time <br /><br />
![image](https://user-images.githubusercontent.com/114446174/193914831-694162a0-8e2b-4fdb-96cd-3c26122ea130.png)

For the analysis, time component is not required. Using Transform option, the data type is changed from “Date\time” to “Date” <br /><br />
![image](https://user-images.githubusercontent.com/114446174/193914887-032c18de-f90e-44b4-b519-521867c1adf7.png)

***2.2 Turn on Column Quality and Column Distribution***  : Column Quality and Column Distribution options are enabled from View section. These options help in scanning the data for any errors or blank values <br /><br />
![image](https://user-images.githubusercontent.com/114446174/193915022-7e3759e4-59a2-480e-9423-7abfec9eb714.png)
![image](https://user-images.githubusercontent.com/114446174/193915165-0ddaf303-5063-483c-8e7c-799e1b086dd0.png)

## **3. Adding Calendar Table** <br />
In order to apply slicer across years, months and days a calendar table is needed. DAX provides a function called “Calendar” which needs start date and end date as input. To create the table with dynamic start and end date, following DAX code is used.

```
Calendar_Table = 
//Calculate Start Date
var start_date = MIN(Sales_Table[OrderDate])

//Calculate End Date
var end_date = MAX(Sales_Table[OrderDate])

RETURN
    // Create a dynamic calendar table
    CALENDAR ( start_date, end_date)
```

In addition, new columns are added which represent year, month, week and day. This is to provide ability to slice the data across multiple periods.<br />

![image](https://user-images.githubusercontent.com/114446174/193915424-d2186f40-594c-443b-8919-55b12beb573f.png)


## **4. Creating data model** <br />
To be able to use all three tables, relationships must be defined across them. Calendar and Sales table are related by Orderkey-Date relationship. Sales and Product Category Table are related by productkey (common among them). The final data model is as shown below.

![image](https://user-images.githubusercontent.com/114446174/193915534-654591df-f16f-4776-935d-177f12742a06.png)

# **Creating visualizations**

***1.	Slicers to choose from Products and Product Categories*** : "ProductName" and "ProductCategory" columns are used in slicers.<br />
![image](https://user-images.githubusercontent.com/114446174/193920450-6177d707-5c16-41a9-97ba-4cdd51de1d79.png)

![image](https://user-images.githubusercontent.com/114446174/193920517-79ce77fd-8054-4e80-8dfc-231abb27755c.png)


***2.	Card - Total Sales card*** : "Product Sales" columns is used in card.<br />

![image](https://user-images.githubusercontent.com/114446174/193916478-adc05489-64b0-4253-997d-2bc80781e3e8.png)

***3.	Card - Average margin*** : A new column is needed to represent margin. Margin can be calculated as follows. Product Margin = Sales – Cost – Tax. The newly calculated column is displayed on the card. <br />

```
Product Margin = 
var margin = CALCULATE(SUM('Sales Table'[SalesAmount]) - SUM('Sales Table'[TotalProductCost]) - SUM('Sales Table'[TaxAmt]))
RETURN
IF(margin = 0, 0, margin)
```
![image](https://user-images.githubusercontent.com/114446174/193916854-8d0f991c-5d68-4e93-aa8b-16b5b6bba6f8.png)
![image](https://user-images.githubusercontent.com/114446174/193916911-0605f513-ae7d-42bd-b73d-25b158a13911.png)


***4.	Card - Products with High sales, High margin and low sales, Low margin*** : To identify product with highest sales as well as margin, ranking of both sales and margin is needed. New column called Sales Rank as shown below <br />
![image](https://user-images.githubusercontent.com/114446174/193917664-440e7eb5-e129-4026-aa75-e85b9e7ba5aa.png)

Further two new columns called Margin Percentage and Margin Rank are created as shown below. <br />
![image](https://user-images.githubusercontent.com/114446174/193921000-d612446c-b4b5-41cb-8c8b-1ef8159753db.png)
![image](https://user-images.githubusercontent.com/114446174/193920891-aa8c39a8-cdc9-4401-8ef8-792e178f8441.png)

Now to combine the Sales rank and Margin rank, another new column called Cumulative Rank is created as shown below <br />
![image](https://user-images.githubusercontent.com/114446174/193920742-1829899e-8bc1-4fcc-88ac-40997e04f857.png)

Top N filter is used to display product with high sales, high margin as well as product with low sales, low margin. <br />
![image](https://user-images.githubusercontent.com/114446174/193921261-eb1d3e4c-ba87-4231-83d3-d5688fa22994.png)
![image](https://user-images.githubusercontent.com/114446174/193921387-8b3f67d2-eaa4-4d91-83c5-0c777bed637c.png)


DAX code available below <br />

```
Sales Rank = 
var bike_found = SEARCH("Bike", 'Product Category'[Product Category],1,0)
RETURN
IF(bike_found =1, RANK.EQ('Product Category'[Product Sales], 'Product Category'[Product Sales], DESC), -1)
```

```
Margin Percentage = DIVIDE('Product Category'[Product Margin],'Product Category'[Product Sales],  0)
```
```
var bike_found = SEARCH("Bike", 'Product Category'[Product Category],1,0)
RETURN
IF(bike_found =1, RANK.EQ('Product Category'[Margin Percentage], 'Product Category'[Margin Percentage], DESC), -1)
```

```
Cumulative Rank = 
IF(OR('Product Category'[Sales Rank] = -1, 'Product Category'[Margin Rank] = -1) = FALSE, 'Product Category'[Sales Rank] * 'Product Category'[Margin Rank], -1)

```


***5.	Pie chart – Contribution to sales by category*** : To identity top contributors, bottom contributors and middle contributors, a new column is created in Product Category table. Using the newly created column, pie chart visual is displayed <br />

![image](https://user-images.githubusercontent.com/114446174/193919264-6139d155-d42b-4e98-8b6d-6126dc0e5619.png)

![image](https://user-images.githubusercontent.com/114446174/193919359-8fa2fcd8-46fc-45e2-9950-4115ee8daed8.png)

```
Sales Contributor Category = 
SWITCH(TRUE(),
'Product Category'[Sales Rank] = -1, "NA",
'Product Category'[Sales Rank] <= 25, "Top Contributor",
'Product Category'[Sales Rank] >= 75, "Bottom Contributor",
"Middle Contributor"
)
```
***6.	Decomposition tree – Sales contribution across brands*** : Product Sales is analyzed by Product Category and Product Name using a Decomposition tree visual. <br />

![image](https://user-images.githubusercontent.com/114446174/193919730-2a7edfcd-54e9-41d4-9de9-454bf89bc23f.png)


***7.	Table – Product wise Sales and Margin*** : Metrix visual along with conditional formatting shows Sales and Margin of each product from the portfolio. <br />

![image](https://user-images.githubusercontent.com/114446174/193920044-1d38eb16-5f7e-4e03-84ca-3e4822b1be53.png)

# **Conclusion**
1. Out of 150 models, the **first 25 products** by sales generate nearly **60% of the sales** for the company. Based on the data, the “**Road 150 Red 48” is the best-selling product** of the company. It generated $1.2 Mn in sales and $0.3 Mn in profit.
2. 50 products fall in bottom contributor category that generate **less than 5% of the sales**. The “**Mountain 300 Black 38**” is product that is generating **low sales and low margin**. Recommendation is to **consider retiring the product**. In case there is inventory of this product, the company may decide to run special discount offers to sell if off quickly.
3. Products in **Middle Contributor** category **generate 36% of the sales**. The details of these products are shared with sales and marketing teams. Recommendation is to identify factors that are working well in case of Top contributors and replicate those to Middle contributors
