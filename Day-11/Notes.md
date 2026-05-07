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

### Superstore Database
[fill in after running checks]

---

## ✅ Day 11 Queries
[fill in as you complete tasks]
