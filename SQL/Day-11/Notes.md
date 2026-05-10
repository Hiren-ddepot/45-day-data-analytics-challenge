# Day 11 — SQL SELECT Basics
## Superstore Sales + Northwind Database

**Date:** 07/05/2026  
**Tools:** Microsoft SQL Server Management Studio (SSMS)  
**Databases:** Superstore Sales + Northwind  

---

## 🗄️ Database Setup

### Northwind Database
Source: SQLite → converted and migrated to SSMS
Migration steps:
1. Downloaded Northwind SQLite database
2. Converted using DB Browser for SQLite
3. Made necessary adjustments for SSMS compatibility
4. Reconstructed tables with proper constraints
5. Fixed identity columns
6. Created analytical SQL views

#### Northwind Tables:
| Table            |  Records|
|------------------|---------|
| Products         |  77 |
| OrderDetails     |  609,283 |
| Categories       |  8 |
| Customers        |  93 |
| Regions          |  4 |
| Territories      |  54 |
| Employees        |  9 |
| Shippers         |  3 |
| Suppliers       |  29 |
| Orders           |  16,282 |

Key observation: OrderDetails is the largest
fact table with 609,283 transactional records.
Customer demographic tables are empty and
excluded from analysis scope.

### Analytical Views Created:

#### Product & Inventory Views:
- Alphabetical list of products
- Current Product List
- ProductDetails_V
- Products by Category
- Products Above Average Price

#### Sales & Revenue Views:
- ProductSales
- Category Sales
- Order Details Extended
- Order Subtotals
- Shipped Orders Sales

#### Order & Transaction Views:
- Orders Qry
- Invoices

#### Customer & Supplier Views:
- Customer and Suppliers by City

#### Project transformation flow:   
SQLite Database
→ SQL Server Migration (SSMS)  
→ Table Reconstruction + Constraints  
→ Data Cleaning + Identity Fixes  
→ Analytical SQL Views Creation  
→ BI-Ready Data Model  

---

### Superstore Database

## 🗄️ Database Setup

### Superstore Star Schema
Built a production-grade star schema from raw CSV:

### Architecture:
FactSales (core fact table)
- Row_ID (PK), Order_ID, Customer_ID (FK)
- Product_ID (FK), Postal_Code (FK)
- Ship_Mode (FK), Order_Date, Ship_Date
- Sales, Quantity, Discount, Profit
- Order_DateKey (FK), Ship_DateKey (FK)

#### Dimension Tables:
DimCustomer  → Customer_ID, Customer_Name, Segment  
DimProduct   → Product_ID, Category, Sub_Category, Product_Name  
DimLocation  → Postal_Code, City, State, Country, Region  
DimShipping  → Ship_Mode  
DimDate      → DateKey, FullDate, Year, Month, MonthName,
               Quarter, Day, WeekDay  

#### Key technical achievements:
- Converted NVARCHAR → DATE for date columns
- Converted Sales → DECIMAL for numeric precision
- Fixed bulk load errors
- Deduplicated DimLocation using ROW_NUMBER()
- Generated DimDate dynamically using recursive CTE
  from MIN/MAX dates in dataset
- Established all foreign key relationships

---

## ✅ Day 11 Queries
[fill in as you complete tasks]
