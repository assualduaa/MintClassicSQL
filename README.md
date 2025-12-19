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

```sql
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
