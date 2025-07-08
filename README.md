
---

# 📊 DSA_Project_June_2025  
## Project Title: Business Intelligence Analysis for Kultra Mega Stores (KMS)

---

## 📌 Table of Contents  
- [Introduction](#introduction)  
- [Project Overview](#project-overview)  
- [Data Source](#data-source)  
- [About the Dataset](#about-the-dataset)  
- [Tools Used](#tools-used)  
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)  
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)  
- [Data Analysis](#data-analysis)  
- [Data Visualisation](#data-visualisation)  
- [Conclusion](#conclusion)  
- [Recommendations](#recommendations)  
- [Author](#author)

---

## 📘 Introduction  
Kultra Mega Stores (KMS) is a retail and wholesale business specializing in office supplies and furniture, headquartered in Lagos, Nigeria. The Abuja division of KMS requested a data analysis to uncover performance insights and drive smarter decision-making using historical order data from 2009 to 2012.

---

## 📊 Project Overview  
The objective of this project is to analyze customer behavior, sales trends, shipping efficiency, and profitability using SQL queries. Insights gained from this analysis will guide business strategy, improve logistics, and enhance customer targeting.

---

## 🔗 Data Source  
The dataset was provided in Excel format and imported into SQL Server. It contains historical order data from 2009 to 2012. Two tables were used:
- **KMS New**: Contains sales, profit, order, and customer details.  
- **Order_Status**: Contains the status of each order (e.g., Returned).

---

## 📦 About the Dataset  
Key columns in the dataset:
- `Order_ID`
- `Order_Date`, `Ship_Date`
- `Product_Name`, `Product_Category`
- `Customer_Name`, `Customer_Segment`
- `Sales`, `Profit`, `Shipping_Cost`
- `Ship_Mode`, `Order_Priority`
- `Region`
- `Status` (from `Order_Status` table)

---

## 🛠 Tools Used  
- **SQL Server Management Studio (SSMS)**  
- **Microsoft Excel** (for preprocessing)

---

## 🧹 Data Cleaning and Preparation  
- Removed null values and duplicates  
- Standardized date formats  
- Ensured relational integrity between `Order_ID` in both tables  
- Validated and corrected data types for numerical fields like `Sales`, `Profit`, and `Shipping_Cost`

---

## 📈 Exploratory Data Analysis (EDA)  
- Examined distribution of sales across regions  
- Analyzed frequency of product categories  
- Evaluated shipping cost patterns by mode  
- Explored order timelines and delivery lags

---

## 🧮 Data Analysis

### 1️⃣ Product Category with Highest Sales
```sql
SELECT TOP 1 Product_Category, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
GROUP BY Product_Category
ORDER BY Total_Sales DESC

Technology had the highest total sales.



---

2️⃣ Top 3 & Bottom 3 Regions by Sales

-- Top 3
SELECT TOP 3 Region, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
GROUP BY Region
ORDER BY Total_Sales DESC

-- Bottom 3
SELECT TOP 3 Region, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
GROUP BY Region
ORDER BY Total_Sales ASC

Top: West, East, Central

Bottom: South, North, Ontario



---

3️⃣ Appliance Sales in Ontario

SELECT SUM(Sales) AS Total_Appliance_Sales
FROM [dbo].[KMS New]
WHERE Region = 'Ontario'
GROUP BY Region

Appliances recorded moderate sales in Ontario.



---

4️⃣ How to Increase Revenue from Bottom 10 Customers

SELECT TOP 10 Customer_Name, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
GROUP BY Customer_Name
ORDER BY Total_Sales ASC

Strategies:

Analyze behavior & purchasing patterns

Personalized promotions and bundling

Feedback forms to understand preferences




---

5️⃣ Most Costly Shipping Method

SELECT TOP 1 Ship_Mode, SUM(Shipping_Cost) AS Total_Cost
FROM [dbo].[KMS New]
GROUP BY Ship_Mode
ORDER BY Total_Cost DESC

Standard Class incurred the highest shipping cost.



---

6️⃣ Most Valuable Customers & Their Products

SELECT TOP 10 Customer_Name, Product_Name, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
GROUP BY Customer_Name, Product_Name
ORDER BY Total_Sales DESC

Key products: Technology and Office Supplies



---

7️⃣ Highest Sales by Small Business Customer

SELECT TOP 1 Customer_Name, Customer_Segment, SUM(Sales) AS Total_Sales
FROM [dbo].[KMS New]
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name, Customer_Segment
ORDER BY Total_Sales DESC

Top Small Business customer showed high engagement.



---

8️⃣ Top Corporate Customer by Order Volume (2009–2012)

SELECT TOP 1 Customer_Name, COUNT(Order_ID) AS Total_Order
FROM [dbo].[KMS New]
WHERE Customer_Segment = 'Corporate' AND Order_Date BETWEEN '2009' AND '2012'
GROUP BY Customer_Name
ORDER BY Total_Order DESC

Most active Corporate customer identified.



---

9️⃣ Most Profitable Consumer Customer

SELECT TOP 1 Customer_Name, SUM(Profit) AS Total_Profit
FROM [dbo].[KMS New]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY Total_Profit DESC

One Consumer customer drove the highest profit.



---

🔟 Customers with Returned Items & Segments

SELECT DISTINCT o.Order_ID, o.Customer_Name, o.Customer_Segment
FROM [dbo].[KMS New] o
JOIN [dbo].[Order_Status] r ON o.Order_ID = r.Order_ID
WHERE r.Status = 'Returned'

All segments had returns; Technology led in return volume.



---

1️⃣1️⃣ Shipping Cost vs. Order Priority

SELECT Order_Priority, Ship_Mode, 
       COUNT(Order_ID) AS Total_Order,
       ROUND(SUM(Sales - Profit), 2) AS Estimated_ShippingCost,
       AVG(DATEDIFF(DAY, Order_Date, Ship_Date)) AS Avg_Ship_Date
FROM [dbo].[KMS New]
GROUP BY Order_Priority, Ship_Mode
ORDER BY Order_Priority, Ship_Mode DESC

Summary of Findings:

Delivery Truck = Cheapest, Slowest

Express Air = Fastest, Most Expensive

Regular Air = Balanced option


> 🚨 Misalignment Detected:
High-priority orders used slow shipping
Low-priority orders used expensive methods
Result: Wasted costs + inefficiencies




---



---

✅ Conclusion

1. Technology led all product categories in sales.


2. West and East were top-performing regions, while South, North, and Ontario lagged.


3. Standard Class shipping dominated in usage and cost.


4. High-value customers typically purchased Technology and Office Supplies.


5. Small Business and Corporate segments were the most profitable.


6. Returned items were significant in the Technology category.


7. Shipping methods were misaligned with order priorities, leading to inefficiencies.




---

💡 Recommendations

1. Increase inventory & marketing in underperforming regions.


2. Engage bottom customers through targeted promotions and bundling.


3. Align shipping methods with order priorities:

Block Express Air for non-urgent orders

Use Delivery Truck for low-priority orders



4. Conduct regular logistics audits.


5. Prioritize quality control, especially in the Technology category.


6. Reinvest savings from optimized shipping into high-priority deliveries.


7. Launch loyalty campaigns for top Corporate and Small Business customers.




---

✍️ Author

Olanrewaju Bukola
Junior Data Analyst | SQL | Excel | Business Intelligence
🔗 GitHub – Klatcy



