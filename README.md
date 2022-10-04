![BA](https://user-images.githubusercontent.com/114446174/193911846-190b0e80-3313-48e6-aee9-7a4e5ffe2f70.jpeg)

# Power-BI-Product-Rationalization

## **Business Context**
An American bike company called Pro Bikes Inc. has two primary sales channels. Those are physical stores and online stores. Within the online channel, the company sells bikes from its own website and from other popular online platforms. The company sells various bikes and bike accessories such as helmets, tires, knee pads, water bottles, etc.  

## **Problem statement**
The company 3 brands of bikes. Each brand has multiple sizes, colors and related varieties. All put together the company sells 125 unique bikes. Managing such high number of SKUs (stock keeping units) is associated with cost in form of inventory technology. The management wants to conduct a product rationalization exercise to fine tune its product portfolio. It has assigned the responsibility to its lead business analyst.

## **Business Analysis**
As a business analyst, I’ve defined the scope of the exercise. It includes creating a mechanism to identify products that make financial sense for the company and identify the ones that do not make financial sense. I’ve made three buckets in which products need to be categorized. The purpose bucketing products is to make data driven recommendations on considering retiring products that are not contributing so that company’s resources can be utilized on other products efficiently. The buckets are as follows.

1.	Top contributor – Products with high sales and high margin
2.	Bottom contributor – Products with low sales and low margin
3.	Middle contributor – Products with decent sales and decent margin


## **Requirement Gathering (User stories)**
The three buckets form part of the requirements. Following are the requirements captured in user story format.

![image](https://user-images.githubusercontent.com/114446174/193912846-391c437a-d6f4-4ba4-ac25-bb49128063ea.png)

## **Data availability**
Since customer information is sensitive, the data resides in company’s secured SQL server. I have been granted a read access to the SQL database to perform analysis. The sales information is available in a fact table named “dbo.FactInternetSales”. In addition to SQL database, I am going to use an excel sheet that contains mapping of products to product categories

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
