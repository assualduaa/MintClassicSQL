# Mint Classics: Inventory & Logistics Optimization 🏎️

## 📌 Project Overview
This project provides a data-driven evaluation of **Mint Classics'** storage facility operations. The company is evaluating the closure of one storage facility to reduce overhead while maintaining an ambitious Service Level Agreement (SLA) of **24-hour shipping**.

### 🎯 Project Goals:
* **Identify Dead Stock:** Pinpoint low-demand products to reduce inventory load.
* **Facility Evaluation:** Determine which warehouse (A, B, C, or D) is the prime candidate for closure.
* **SLA Compliance Audit:** Analyze current shipping performance against the 24-hour delivery promise.

---

## 📊 Data Analysis & Findings

### 1. Low-Demand Product Identification
We cross-validated demand with current inventory to find products with low sales velocity. These are strong candidates for discontinuation to free up warehouse capacity.


-- Identifying top 10 products with high stock but low demand
SELECT 
    p.productCode, 
    p.productName, 
    p.quantityInStock,
    low_demand.total_units_sold
FROM products p
JOIN (
    SELECT productCode, SUM(quantityOrdered) AS total_units_sold
    FROM orderdetails
    GROUP BY productCode
    ORDER BY total_units_sold ASC
    LIMIT 10
) low_demand ON p.productCode = low_demand.productCode;

2. Warehouse Closure Analysis
Based on physical load and outbound demand, Warehouse D was identified as the least utilized facility.

Warehouse,Inventory Volume,Sales Demand Link
Warehouse A,High,High
Warehouse B,Moderate,Moderate
Warehouse C,Moderate,Moderate
Warehouse D,Lowest,Lowest

Conclusion: Warehouse D is the primary candidate for closure as it handles the minimum percentage of the total business volume.

3. SLA Audit: The 24-Hour Reality Check
The current SLA states: "Orders must ship within 24 hours." Our analysis reveals a significant performance gap.

-- Calculating the percentage of orders meeting the 24-hour SLA
SELECT 
    ROUND(
        SUM(CASE WHEN TIMESTAMPDIFF(HOUR, o.orderDate, o.shippedDate) <= 24 THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 
        2
    ) AS sla_met_percentage
FROM orders o
WHERE o.shippedDate IS NOT NULL;

Key Metric:
SLA Met Rate: 16.03%

SLA Breach Rate: 83.97%



## 📊 Data Analysis & Findings

### 1. Low-Demand Product Identification
We cross-validated demand with current inventory to find products with low sales velocity. These are strong candidates for discontinuation to free up warehouse capacity.

sql
-- Identifying top 10 products with high stock but low demand
SELECT 
    p.productCode, 
    p.productName, 
    p.quantityInStock,
    low_demand.total_units_sold
FROM products p
JOIN (
    SELECT productCode, SUM(quantityOrdered) AS total_units_sold
    FROM orderdetails
    GROUP BY productCode
    ORDER BY total_units_sold ASC
    LIMIT 10
) low_demand ON p.productCode = low_demand.productCode;


2. Warehouse Closure AnalysisBased on physical load and outbound demand, Warehouse D was identified as the least utilized facility.WarehouseInventory VolumeSales Demand LinkWarehouse AHighHighWarehouse BModerateModerateWarehouse CModerateModerateWarehouse DLowestLowestConclusion: Warehouse D is the primary candidate for closure as it handles the minimum percentage of the total business volume.⚡ SLA Audit: The 24-Hour Reality CheckThe current SLA states: "Orders must ship within 24 hours." Our analysis reveals a significant performance gap.SQL-- Calculating the percentage of orders meeting the 24-hour SLA
SELECT 
    ROUND(
        SUM(CASE WHEN TIMESTAMPDIFF(HOUR, o.orderDate, o.shippedDate) <= 24 THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 
        2
    ) AS sla_met_percentage
FROM orders o
WHERE o.shippedDate IS NOT NULL;

📉 Key Metric:SLA Met Rate: 16.03%SLA Breach Rate: 83.97%

💡 ## Final Suggestions & Conclusion
# Inventory Optimization:
10 low-performing products are recommended for discontinuation, which would immediately lower space requirements.Facility Closure: Warehouse D can be closed with minimal risk, provided its high-velocity stock is moved to Warehouse A or B.
Critical SLA Warning: Mint Classics is currently failing its 24-hour promise (only 16% compliance). Closing a facility without reorganization will worsen this.RecommendationsPrioritize High-Velocity Stock: High-demand items must be relocated to the dispatch points nearest to transportation hubs.Improve Workflow: Before closing any facility, the internal picking and packing process must be optimized to address the existing 84% SLA breach rate.Outcome: By liquidating "dead stock" and centralizing high-demand items, Mint Classics can successfully close Warehouse D, save on operational costs, and actually improve fulfillment times.


🛠️ Tools UsedLanguage: SQL (MySQL)Analysis: Lead-to-Sale Funnel Analysis, Fulfillment Time Tracking, Data Modeling.Documentation: Markdown.
