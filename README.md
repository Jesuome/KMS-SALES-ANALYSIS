# KULTRA MEGA STORES ORDER AND SALES ANALYSIS

## TABLE OF CONTENTS

- [PROJECT OVERVIEW](#project-overview)
- [DATA SOURCES](#data-sources)
- [TOOL](#tool)
- [DATA CLEANING AND PREPARATION](#data-cleaning-and-preparation)
- [EXPLORATORY DATA ANALYSIS](#exploratory-data-analysis)
- [ANALYSIS](#analysis)
- [RESULTS AND FINDINGS](#results-and-findings)
- [RECOMMENDATIONS](recommendations)

### PROJECT OVERVIEW

This data analysis project aim to provides insights into the sales perfomance of **KULTRA MEGA STORES** (KMS) Abuja division. By analyzing aspects of the order data, we seek to Identify trends, make data driven recommendation, and gain a deeper understanding of the company's perfomance in terms of Sales.

### DATA SOURCES

Sales Data: the primary dataset used for this analysis is the "KMS_case_study_Excel" file, containing detailed information about each sale made by the company.

### TOOL

 - SQL Server [Download here](https://SQLServer.com)
    - Data Cleaning and Data Prepocessing
    - Data Analysis
    - Reports

  ### DATA CLEANING AND PREPARATION

  The initial Data Preparation phase, we performed the following tasks:
  1. Creating Database
  2. Importing of Data and Inspection
  3. Data Cleaning and Formatting
      - Change Datatype of some columns
      - Handling Duplicate values on Order ID
      - Make order ID primary key
      - Drop column

### EXPLORATORY DATA ANALYSIS

EDA involves exploring the order data to answer key questions, such as:
- Which product category had the highest sales?
- What are the Top 3 and Bottom 3 regions in terms of sales?
- What were the total sales of appliances in Ontario?
- Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers
- KMS incurred the most shipping cost using which shipping method?
- Who are the most valuable customers, and what products or services do they typically purchase?
- Which small business customer had the highest sales?
- Which Corporate Customer placed the most number of orders in 2009 – 2012?
- Which consumer customer was the most profitable one?
- Which customer returned items, and what segment do they belong to?
- If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority?

### ANALYSIS
### SQL Analysis Queries

You can view the full list of the SQL queries 



#### Case Scenario 1
#### 1.  Which product category had the highest sales?

```sql
SELECT TOP 1 Product_Category, SUM(SALES) AS TOTAL_SALES
FROM [KMS Sql Case Study 1]
GROUP BY Product_Category
ORDER BY TOTAL_SALES DESC
```

**Insight: Top Performing Product Category**

Analysis shows that the Technology product category generated the highest total sales, amounting to ₦3,734,232.38.

This suggests that Technology products are a key revenue driver for KMS and may benefit from continued investment in marketing, customer targeting, and inventory optimization to sustain and grow sales in this category.



#### 2. What are the Top 3 and Bottom 3 regions in terms of sales?

```sql
----TOP 3 REGIONS IN TERMS OF SALES-
SELECT TOP 3 Region, SUM (SALES) AS TOTAL_SALES
FROM [KMS Sql Case Study 1]
GROUP BY Region
ORDER BY TOTAL_SALES DESC

----BOTTOM 3 REGIONS IN TERMS OF SALES-
SELECT TOP 3 Region, SUM (SALES) AS TOTAL_SALES
FROM [KMS Sql Case Study 1]
GROUP BY Region
ORDER BY TOTAL_SALES ASC
```

**Insight: Regional Sales Performance**

An analysis of total sales by region reveals a significant gap between the top- and bottom-performing areas:
- Top 3 Regions by Sales:
  - West – ₦2,373,731.44
  - Ontario – ₦2,028,782.78
  - Prarie – ₦1,846,466.90
- Bottom 3 Regions by Sales:
  - Nunavut – ₦86,241.69
  - Northwest Territories – ₦521,175.20
  - Yukon – ₦674,757.09

This regional distribution suggests that strategic efforts (like improved marketing, localized offers, or better logistics) could be targeted at the lower-performing regions to boost overall revenue.


#### 3.  What were the total sales of appliances in Ontario?

```sql
SELECT 
	Region AS Region,
	Product_Sub_Category,
	SUM(SALES) AS TOTAL_SALES
FROM [KMS Sql Case Study 1]
WHERE Product_Sub_Category = 'APPLIANCES'
	AND Region = 'ONTARIO'
GROUP BY Region, Product_Sub_Category
```

**Insights**: SQL Analysis indicates that Appliances generated a total sales value of ₦187,490.6199 in Ontario region


#### 4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers

```sql
ALTER TABLE [KMS Sql Case Study 1]
ADD REVENUE MONEY;

UPDATE [KMS Sql Case Study 1]
SET REVENUE = Unit_Price * Order_Quantity

SELECT TOP 10
		Order_ID, Customer_Name, Customer_Segment, Region, Product_Category, Product_Sub_Category, Product_Name, Order_Priority, Order_Quantity, Ship_Mode, REVENUE,
		SUM(Sales) AS TOTAL_REVENUE
FROM [KMS Sql Case Study 1]
WHERE REVENUE IS NOT NULL
GROUP BY Order_ID, Customer_Name, Customer_Segment, Region, Product_Category, Product_Sub_Category, Product_Name, Order_Priority, Order_Quantity, Ship_Mode, REVENUE
ORDER BY TOTAL_REVENUE ASC
```

**Insight from Bottom 10 Customers Analysis**

An in-depth analysis of KMS’s bottom 10 customers (by revenue) reveals the following key points:

- Most placed critical-priority orders but were shipped using Regular Air instead of Express Air, increasing the risk of delayed or damaged deliveries.
- The majority of orders were placed only once or twice, between 2009–2012, suggesting low engagement or dissatisfaction.
- The affected products mostly fall under the Office Supplies category, which are typically urgently needed and sensitive to delivery speed and handling.
- This pattern suggests that mismatched shipping methods and poor delivery experiences may have directly contributed to low customer retention and revenue.

Advice to Increase Revenue from Bottom Customers
- Align Shipping Method with Order Priority: Ensure that critical orders are always fulfilled using Express Air to meet urgency expectations and improve customer satisfaction.
- Improve Delivery Experience for Office Supplies: Analyze delivery feedback and complaints specifically related to Office Supplies. Identify any recurring delays or damage issues, and work with logistics to streamline handling.
- Re-Engage Inactive Customers: Reach out to these customers to understand why they stopped ordering. Use insights from their feedback to tailor special offers, improved service, or personalized outreach.
- Automate Shipping Logic: Implement a system that automatically selects the appropriate shipping method based on order priority to reduce manual errors and improve consistency.


#### 5. KMS incurred the most shipping cost using which shipping method?

```sql
SELECT TOP 1 
	Ship_Mode,
	SUM(Shipping_Cost) AS TOTALCOST		
FROM [KMS Sql Case Study 1]
GROUP By Ship_Mode
ORDER BY TOTALCOST DESC
```

**Insight: Highest Shipping Cost by Ship Mode**

KMS incurred the highest total shipping cost of ₦33,937.92 using the  Delivery Truck shipping method.

This highlights the need to evaluate:
- Whether this shipping method is being used efficiently,
- If the cost is justified by order priority or value, and
- Whether there are opportunities to optimize logistics and reduce expenses.


#### Case Scenario 2
#### 6. Who are the most valuable customers, and what products or services do they typically purchase?

```sql
SELECT TOP 10
	Customer_Name,
	Customer_Segment,
	Product_Category,
	Product_Sub_Category,
	Product_Name,
	SUM(Sales) AS TOTAL_SPENT
FROM [KMS Sql Case Study 1]
GROUP BY Customer_Name, Customer_Segment, Product_Category, Product_Sub_Category, Product_Name
ORDER BY TOTAL_SPENT DESC
```

**Insight: Top 10 Most Valuable Customers and Their Preferred Products**

Based on sales analysis in SQL, the top 10 highest-value customers and they consistently purchase high-value technology and office equipment, including:

|S/N|**CUSTOMER NAME**|**PRODUCT NAME**|
|---|-----------------|----------------|
|1.|Emily Phan|Polycom ViewStation™ ISDN Videoconferencing Unit|
|2.|Clytie Kelty|Canon PC940 Copier|
|3.|Karen Carlisle|Canon Image Class D660 Copier|
|4.|Nick Crebassa|Hewlett-Packard Business Color Inkjet 3000 Series Printers|
|5.|Parhena Norris|Canon imageCLASS 2200 Advanced Copier|
|6.|Deborah Brumfield|Hewlett Packard LaserJet 3310 Copier|
|7.|Sally Matthias|Riverside Palais Royal Lawyers Bookcase (Royale Cherry Finish)|
|8.|Seth Vernon|Canon Image Class D660 Copier|
|9.|Raymond Book|Hewlett Packard LaserJet 3310 Copier|
|10.|Tony Chapman|Canon imageCLASS 2200 Advanced Copier|

**Key Observation**:

Most top customers show a preference for high-end printing, imaging, and videoconferencing equipment, suggesting a strong opportunity for:

- Premium product promotions
- Long-term service contracts
- Customer loyalty or retention programs


#### 7.  Which small business customer had the highest sales?

```sql
SELECT TOP 1 
	Customer_Segment AS Customer_Segment,
	Customer_Name, 
	SUM(Sales) AS TOTAL_SALES
FROM [KMS Sql Case Study 1]
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Segment, Customer_Name
ORDER BY TOTAL_SALES DESC
```

**Insight: Top Customer in Small Business Segment**

Based on the analysis, Clytie Kelty is the highest-value customer in the Small Business segment, with a total sales value of ₦50,664.20.

This highlights her as a key client within this segment and may present an opportunity for targeted engagement or loyalty incentives.


#### 8. Which Corporate Customer placed the most number of orders in 2009 – 2012?

```sql
SELECT TOP 2
	Customer_Segment AS Customer_Segment,
	Customer_Name,
	COUNT(Order_Quantity) AS TOTAL_ORDERS
FROM [KMS Sql Case Study 1]
WHERE Customer_Segment = 'Corporate'
		AND YEAR(Order_Date) BETWEEN 2009 AND 2012
GROUP BY Customer_Segment, Customer_Name
ORDER BY TOTAL_ORDERS DESC
```

**Insight: Top Corporate Customers by Order Volume (2009–2012)**

Based on the analysis, two customers in the Corporate segment placed the highest number of orders between 2009 and 2012, with 18 orders each:
1. Adam Hart
2. Roy Skaria

This consistent purchasing behavior highlights them as valuable and engaged clients in the corporate segment between 2009 and 2012. KMS could consider re-engagement strategies or loyalty incentives to strengthen long-term relationships with such high-frequency customers.


#### 9. Which consumer customer was the most profitable one?

```sql
SELECT TOP 1
	Customer_Segment AS Customer_Segment,
	Customer_Name,
	SUM(Profit) AS TOTAL_PROFIT
FROM [KMS Sql Case Study 1]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Segment, Customer_Name
ORDER BY TOTAL_PROFIT DESC
```

**Insight: Most Profitable Customer in the Consumer Segment**

Based on the analysis, Emily Phan is the most profitable customer in the Consumer segment, with a total profit of ₦32,985.54.

This indicates a strong customer relationship and purchasing behavior that significantly contributes to overall profitability. Emily Phan could be considered a key customer for exclusive offers, targeted retention strategies, or premium loyalty programs.


#### 10. Which customer returned items, and what segment do they belong to?

```sql
SELECT *FROM Order_Status

SELECT [KMS Sql Case Study 1].Order_ID, [KMS Sql Case Study 1].Row_ID, [KMS Sql Case Study 1].Order_Priority,
	[KMS SqL Case Study 1].Order_Quantity, [KMS SqL Case Study 1].Customer_Name, [KMS SqL Case Study 1].Customer_Segment, [KMS SqL Case Study 1].Product_Category, 
	[KMS SqL Case Study 1].Product_Sub_Category,[KMS SqL Case Study 1].Product_Name, Order_Status.Status
FROM [KMS Sql Case Study 1]
JOIN Order_Status
ON Order_Status.Order_ID = [KMS Sql Case Study 1].Order_ID

SELECT TOP 20 [KMS Sql Case Study 1].Order_ID, [KMS Sql Case Study 1].Row_ID, [KMS Sql Case Study 1].Order_Priority,
	[KMS SqL Case Study 1].Order_Quantity, [KMS SqL Case Study 1].Customer_Name, [KMS SqL Case Study 1].Customer_Segment, [KMS SqL Case Study 1].Product_Category, 
	[KMS SqL Case Study 1].Product_Sub_Category,[KMS SqL Case Study 1].Product_Name, Order_Status.Status
FROM [KMS Sql Case Study 1]
JOIN Order_Status
ON Order_Status.Order_ID = [KMS Sql Case Study 1].Order_ID
ORDER BY Order_Quantity desc
```

**Insight**:

A total of 572 customers returned items. This cut across various segments, including Consumer, Home Office, Corporate, and Small Business.
Identifying return patterns by segment can help KMS investigate reasons for returns and take steps to improve product satisfaction or delivery processes.

The second query returns back the top 20 customers that returned items base on Order quantity.


#### 11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority?

```sql
SELECT 
	Order_Priority,
	Ship_Mode, 
	COUNT(Order_ID) AS NUMBERS_OF_ORDERS,
	SUM(Shipping_Cost) AS TOTAL_SHIPPINGCOST,
	SUM(Sales - Profit) AS ESTIMATED_SHIPPING_COST,
	AVG(DATEDIFF(DAY, Order_Date, Ship_Date)) AS AVG_SHIP_DAYS 
FROM [KMS Sql Case Study 1]
GROUP BY Order_Priority, Ship_Mode
ORDER BY Order_Priority, Ship_Mode DESC
```

**Report**: KMS did not spend appropriately on shipping based on order priority
 
**Shipping Cost vs Order Priority Misalignment**

**Analysis Summary**

The analysis reveals a misalignment between shipping methods and order priorities, indicating that KMS is not spending appropriately on shipping in relation to urgency levels:

1. Critical Orders Shipped with Inappropriate Modes:
 - Several critical-priority orders were often shipped using Regular Air or Delivery Truck, instead of Express Air, risking delays.
 - This increases the risk of late deliveries or product damage, which can lead to customer dissatisfaction and loss of future sales.
 - The company appears to be cutting costs at the expense of service quality.

2. Overspending on Low Priority Orders
- Low-priority orders were mostly shipped via Regular Air or Express Air, instead of using more cost-effective options like Delivery Truck.
- This reflects unnecessary overspending on non-urgent shipments, reducing cost efficiency.

3. Medium and High-priority orders were not consistently matched with cost-effective methods.

**Advice**

To improve shipping efficiency and customer satisfaction, KMS should:

- Use Express Air for all critical and some high-priority orders.
- Use Regular Air for high-priority orders where timely delivery is essential but not critical.
- Use Delivery Truck for low-priority orders to minimize cost on non-urgent deliveries.

This alignment will help the company optimize logistics spending without compromising on delivery performance, improving both customer experience and operational cost control.


### RESULTS AND FINDINGS

- Top Customers:
  - Emily Phan (Consumer) had the highest profit.
  - Clytie Kelty (Small Business) had the highest sales.
  - Adam Hart and Roy Skaria (Corporate) placed the most orders (18 each).
- Best-Selling Category:
  - Technology had the highest total sales (₦3.7M+).
- Top Regions by Sales:
  - West, Ontario, and Prarie led in revenue.
  - Nunavut, Yukon, and Northwest Territories had the lowest sales.
- Shipping & Cost Issues:
  - Critical orders were often sent with the wrong shipping method.
  - Low-priority orders used costly methods, causing overspending.
- Customer Returns:
  - 572 customers returned items across all segments.
  - Needs further review to reduce return rates.

### RECOMMENDATIONS:

Based on the Analysis, we recommend the following actions:
- Align shipping method with order priority
- Improve service for top customers and high-return segments
- Focus marketing on underperforming regions
- Maximize sales from high-performing product categories







