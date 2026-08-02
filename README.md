SQL E-Commerce Orders Analysis
A SQL-based data analytics project that extracts business insights from a 1,200-record e-commerce orders dataset — covering data filtering, sorting, grouping, and aggregation.

Project Overview
This project was completed as part of the DecodeLabs Data Analytics Industrial Training Kit (Project 3: SQL Data Analysis). The goal was to move beyond simply "viewing" a spreadsheet and instead use structured SQL queries to filter, group, and aggregate raw transactional data into actionable business intelligence.

Dataset
File: Dataset_for_Data_Analytics.xlsx Rows: 1,200 orders Columns (14):

Column	Description
OrderID	Unique order identifier
Date	Order date
CustomerID	Unique customer identifier
Product	Item purchased (Monitor, Phone, Tablet, Chair, Printer, Laptop, Desk)
Quantity	Units ordered
UnitPrice	Price per unit
ShippingAddress	Delivery address
PaymentMethod	Debit Card, Credit Card, Online, Gift Card, Cash
OrderStatus	Shipped, Delivered, Cancelled, Returned, Pending
TrackingNumber	Shipment tracking ID
ItemsInCart	Number of items in cart at checkout
CouponCode	SAVE10, FREESHIP, WINTER15, or none
ReferralSource	Instagram, Facebook, Google, Email, Referral
TotalPrice	Final order value
The data was loaded into a SQLite database (table: orders) so standard SQL could be run against it directly.

Tools & Skills Used
SQL fundamentals (SELECT, FROM)
Filtering with WHERE (equality, comparison, compound conditions)
Sorting with ORDER BY
Grouping with GROUP BY
Aggregate functions: COUNT(), SUM(), AVG()
Filtering aggregated groups with HAVING
Subqueries for percentage-of-total calculations
SQLite (via Python)
Repository Contents
├── README.md                                   # This file
├── Project_3_Queries.sql                       # All SQL queries used in the analysis
├── Project_3_SQL_Data_Analysis_Report.pdf      # Full write-up: queries, results, and insights
└── Dataset_for_Data_Analytics.xlsx             # Source dataset
Sample Queries
Revenue by product:

SELECT Product, SUM(TotalPrice) AS TotalRevenue
FROM orders
GROUP BY Product
ORDER BY TotalRevenue DESC;
High-value products using HAVING:

SELECT Product, COUNT(*) AS OrdersCount, ROUND(SUM(TotalPrice),2) AS Revenue
FROM orders
GROUP BY Product
HAVING SUM(TotalPrice) > 150000
ORDER BY Revenue DESC;
Each product's share of total revenue (subquery):

SELECT Product,
       ROUND(SUM(TotalPrice),2) AS Revenue,
       ROUND(100.0 * SUM(TotalPrice) / (SELECT SUM(TotalPrice) FROM orders), 2) AS PctOfTotalRevenue
FROM orders
GROUP BY Product
ORDER BY Revenue DESC;
See Project_3_Queries.sql for the complete set of 14 queries.

Key Findings
Laptop and Monitor are the top two products by total revenue.
Average order value varies meaningfully across payment methods, suggesting checkout options influence basket size.
A notable share of orders are Cancelled or Returned — worth investigating for fulfillment or quality issues.
Referral channels differ in average order value, not just order volume — the highest-traffic channel isn't always the most valuable.
The top 10 customers by spend represent a disproportionate share of revenue relative to their share of orders, consistent with an 80/20 (Pareto) distribution.
