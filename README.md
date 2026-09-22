# 🚀 Data Transformer — Advanced SQL Project

> **A professional MySQL project for learning data transformation, joins, subqueries, date functions, string functions, window functions, and conditional analysis.**

---

## 📌 Project Overview

**Data Transformer** is a practical SQL project that simulates a **Corporate Data Analysis System**.

The project works with three main datasets:

- 👤 **Customers** — customer information
- 🛒 **Orders** — sales/order transactions
- 👨‍💼 **Employees** — employee and salary information

The goal is to transform raw database records into useful information using advanced SQL queries.

---

## 🎯 Project Objectives

By completing this project, you will practice:

| # | Skill | SQL Concept |
|---|---|---|
| 1 | 🔗 Combine tables | JOINs |
| 2 | 🔍 Filter using another query | Subqueries |
| 3 | 📅 Work with dates | Date Functions |
| 4 | 🔤 Transform text | String Functions |
| 5 | 📊 Analyze totals | Aggregate Functions |
| 6 | 📈 Create rankings | Window Functions |
| 7 | 🧠 Apply conditions | CASE Expression |

---

# 🗂️ Database Structure

## 👤 1. Customers Table

Stores customer registration and contact information.

| Column | Data Type | Description |
|---|---|---|
| `CustomerID` | INT | Unique customer ID |
| `FirstName` | VARCHAR | Customer first name |
| `LastName` | VARCHAR | Customer last name |
| `Email` | VARCHAR | Customer email |
| `RegistrationDate` | DATE | Registration date |

### 📄 Sample Data

| CustomerID | FirstName | LastName | Email | RegistrationDate |
|---:|---|---|---|---|
| 1 | John | Doe | john.doe@email.com | 2022-03-15 |
| 2 | Jane | Smith | jane.smith@email.com | 2021-11-02 |

---

## 🛒 2. Orders Table

Stores customer order and transaction information.

| Column | Data Type | Description |
|---|---|---|
| `OrderID` | INT | Unique order ID |
| `CustomerID` | INT | Customer reference |
| `OrderDate` | DATE | Order date |
| `TotalAmount` | DECIMAL | Order amount |

### 📄 Sample Data

| OrderID | CustomerID | OrderDate | TotalAmount |
|---:|---:|---|---:|
| 101 | 1 | 2023-07-01 | 150.50 |
| 102 | 2 | 2023-07-03 | 200.75 |

---

## 👨‍💼 3. Employees Table

Stores employee and salary information.

| Column | Data Type | Description |
|---|---|---|
| `EmployeeID` | INT | Unique employee ID |
| `FirstName` | VARCHAR | Employee first name |
| `LastName` | VARCHAR | Employee last name |
| `Department` | VARCHAR | Employee department |
| `HireDate` | DATE | Hiring date |
| `Salary` | DECIMAL | Employee salary |

### 📄 Sample Data

| EmployeeID | FirstName | LastName | Department | HireDate | Salary |
|---:|---|---|---|---|---:|
| 1 | Mark | Johnson | Sales | 2020-01-15 | 50000.00 |
| 2 | Susan | Lee | HR | 2021-03-20 | 55000.00 |

---

# 🔗 Table Relationship

```text
┌───────────────┐
│   Customers   │
├───────────────┤
│ CustomerID PK │
└───────┬───────┘
        │
        │ CustomerID
        │
        ▼
┌───────────────┐
│    Orders     │
├───────────────┤
│ OrderID PK    │
│ CustomerID FK │
└───────────────┘

┌───────────────┐
│   Employees   │
├───────────────┤
│ EmployeeID PK │
│ Salary        │
└───────────────┘
```

**Relationship:** `Customers.CustomerID` → `Orders.CustomerID`

---
## 🎥 Video Demonstration

[![Watch Video](https://img.shields.io/badge/🎥-Watch_Video-red?style=for-the-badge)]([video](https://drive.google.com/file/d/1Po6kM7qdcSTPI-Gy0muNxCA4l0_3NoVt/view?usp=sharing))

# 🧪 SQL Query Tasks

## 🔗 01. INNER JOIN — Orders + Customers

### 💡 Query Purpose
Retrieve only customers who have placed an order.

```sql
SELECT
    o.OrderID,
    c.CustomerID,
    c.FirstName,
    c.LastName,
    c.Email,
    o.OrderDate,
    o.TotalAmount
FROM Orders AS o
INNER JOIN Customers AS c
    ON o.CustomerID = c.CustomerID;
```
<img width="961" height="135" alt="image" src="https://github.com/user-attachments/assets/401101c2-2ee8-4c1c-b554-159fe3132829" />


### 🧠 Explanation

- `INNER JOIN` returns matching records from both tables.
- `o.CustomerID = c.CustomerID` connects the two tables.
- Customers without orders are not included.

---

## 👥 02. LEFT JOIN — All Customers + Orders

### 💡 Query Purpose
Show every customer, including customers who have not placed an order.

```sql
SELECT
    c.CustomerID,
    c.FirstName,
    c.LastName,
    o.OrderID,
    o.OrderDate,
    o.TotalAmount
FROM Customers AS c
LEFT JOIN Orders AS o
    ON c.CustomerID = o.CustomerID;
```
<img width="985" height="125" alt="image" src="https://github.com/user-attachments/assets/dd756db4-a821-41b3-9745-130b88b006da" />


### 🧠 Explanation

- `LEFT JOIN` keeps all rows from `Customers`.
- If a customer has no order, order columns return `NULL`.

---

## 🔄 03. RIGHT JOIN — All Orders + Customers

### 💡 Query Purpose
Retrieve all orders and their corresponding customer information.

```sql
SELECT
    o.OrderID,
    o.OrderDate,
    o.TotalAmount,
    c.CustomerID,
    c.FirstName,
    c.LastName
FROM Customers AS c
RIGHT JOIN Orders AS o
    ON c.CustomerID = o.CustomerID;
```
<img width="862" height="104" alt="image" src="https://github.com/user-attachments/assets/b925dd2d-1e49-4d85-be16-8ad7639e0d05" />

### 🧠 Explanation

- `RIGHT JOIN` keeps every record from the right-side table.
- Here, the right-side table is `Orders`.

---

## 🌐 04. FULL OUTER JOIN — Customers + Orders

### 💡 Query Purpose
Retrieve all customers and all orders, whether they match or not.

> ⚠️ **MySQL does not directly support `FULL OUTER JOIN`.** Use `LEFT JOIN + RIGHT JOIN + UNION`.

```sql
SELECT
    c.CustomerID,
    c.FirstName,
    c.LastName,
    o.OrderID,
    o.OrderDate,
    o.TotalAmount
FROM Customers AS c
LEFT JOIN Orders AS o
    ON c.CustomerID = o.CustomerID

UNION

SELECT
    c.CustomerID,
    c.FirstName,
    c.LastName,
    o.OrderID,
    o.OrderDate,
    o.TotalAmount
FROM Customers AS c
RIGHT JOIN Orders AS o
    ON c.CustomerID = o.CustomerID;
```
<img width="963" height="113" alt="image" src="https://github.com/user-attachments/assets/3d358c2a-db32-49a0-9ce4-cac65770db5e" />

### 🧠 Explanation

- `LEFT JOIN` gets all customers.
- `RIGHT JOIN` gets all orders.
- `UNION` combines both result sets.

---

# 🔍 05. Subquery — Orders Above Average

### 💡 Query Purpose
Find customers whose order amount is greater than the average order amount.

```sql
SELECT
    c.CustomerID,
    c.FirstName,
    c.LastName,
    o.OrderID,
    o.TotalAmount
FROM Customers AS c
INNER JOIN Orders AS o
    ON c.CustomerID = o.CustomerID
WHERE o.TotalAmount > (
    SELECT AVG(TotalAmount)
    FROM Orders
);
```
<img width="812" height="102" alt="image" src="https://github.com/user-attachments/assets/a7a0f1b6-270f-4dfa-87ad-0e8a3e6318d9" />

### 🧠 Explanation

The inner query calculates the average order amount:

```sql
SELECT AVG(TotalAmount)
FROM Orders;
```
<img width="756" height="88" alt="image" src="https://github.com/user-attachments/assets/a0ede952-5d89-43f5-9c43-90c7cdecafa3" />

The outer query then finds orders greater than that average.

---

# 💰 06. Subquery — Salary Above Average

### 💡 Query Purpose
Find employees whose salary is higher than the average employee salary.

```sql
SELECT
    EmployeeID,
    FirstName,
    LastName,
    Department,
    Salary
FROM Employees
WHERE Salary > (
    SELECT AVG(Salary)
    FROM Employees
);
```
<img width="824" height="94" alt="image" src="https://github.com/user-attachments/assets/27d4a726-6bbc-48fa-9578-18dba6f15a3a" />

### 🧠 Explanation

- `AVG(Salary)` calculates the average salary.
- The outer query returns employees earning above that average.

---

# 📅 07. Extract Year and Month

### 💡 Query Purpose
Extract the year and month from `OrderDate`.

```sql
SELECT
    OrderID,
    OrderDate,
    YEAR(OrderDate) AS OrderYear,
    MONTH(OrderDate) AS OrderMonth
FROM Orders;
```
<img width="849" height="103" alt="image" src="https://github.com/user-attachments/assets/42b46994-4e63-4ba1-b4b5-5bab7599e38f" />

### 🧠 Explanation

- `YEAR()` extracts the year.
- `MONTH()` extracts the month number.

---

# 📆 08. Calculate Days Since Order

### 💡 Query Purpose
Calculate the number of days between the order date and today's date.

```sql
SELECT
    OrderID,
    OrderDate,
    DATEDIFF(CURRENT_DATE, OrderDate) AS DaysDifference
FROM Orders;
```
<img width="796" height="105" alt="image" src="https://github.com/user-attachments/assets/ed4af24d-9d39-425c-bd60-79c277d2b338" />

### 🧠 Explanation

`DATEDIFF()` calculates the difference between two dates.

---

# 🗓️ 09. Format Order Date

### 💡 Query Purpose
Display the order date in a more readable format.

```sql
SELECT
    OrderID,
    OrderDate,
    DATE_FORMAT(OrderDate, '%d-%b-%Y') AS FormattedDate
FROM Orders;
```
<img width="928" height="96" alt="image" src="https://github.com/user-attachments/assets/d489ce0a-a02b-4809-a4b6-0de3bb4f6dbb" />

### 🧠 Explanation

Example:

```text
2023-07-01 → 01-Jul-2023
```

---

# 👤 10. CONCAT — Create Full Name

### 💡 Query Purpose
Combine first name and last name into one `FullName` column.

```sql
SELECT
    CustomerID,
    CONCAT(FirstName, ' ', LastName) AS FullName
FROM Customers;
```
<img width="787" height="97" alt="image" src="https://github.com/user-attachments/assets/40fd2135-cffe-4368-a489-a6aa9fc850fc" />

### 🧠 Explanation

`CONCAT()` combines multiple strings into one string.

Example:

```text
John + Doe → John Doe
```

---

# ✏️ 11. REPLACE — Replace Text

### 💡 Query Purpose
Replace `John` with `Jonathan`.

```sql
SELECT
    FirstName,
    REPLACE(FirstName, 'John', 'Jonathan') AS UpdatedName
FROM Customers;
```
<img width="778" height="100" alt="image" src="https://github.com/user-attachments/assets/0c117028-4a2a-45ff-b0df-d48b2c9ef676" />

### 🧠 Explanation

`REPLACE()` searches for specific text and replaces it with new text.

---

# 🔠 12. UPPER + LOWER — Change Text Case

### 💡 Query Purpose
Convert first names to uppercase and last names to lowercase.

```sql
SELECT
    UPPER(FirstName) AS FirstName_Upper,
    LOWER(LastName) AS LastName_Lower
FROM Customers;
```
<img width="1037" height="96" alt="image" src="https://github.com/user-attachments/assets/69d1ae5d-eb85-49f2-9194-6dfd962d1c48" />

### 🧠 Explanation

- `UPPER()` → converts text to uppercase.
- `LOWER()` → converts text to lowercase.

Example:

```text
John Doe → JOHN doe
```

---

# ✂️ 13. TRIM — Remove Extra Spaces

### 💡 Query Purpose
Remove unnecessary spaces from email addresses.

```sql
SELECT
    Email,
    TRIM(Email) AS CleanEmail
FROM Customers;
```
<img width="1067" height="104" alt="image" src="https://github.com/user-attachments/assets/a7928da0-ccda-4196-94c5-61790efdab80" />

### 🧠 Explanation

`TRIM()` removes leading and trailing spaces.

---

# 📈 14. Running Total — SUM() OVER()

### 💡 Query Purpose
Calculate the cumulative total of order amounts.

```sql
SELECT
    OrderID,
    OrderDate,
    TotalAmount,
    SUM(TotalAmount) OVER (
        ORDER BY OrderDate, OrderID
    ) AS RunningTotal
FROM Orders;
```
<img width="1000" height="110" alt="image" src="https://github.com/user-attachments/assets/c4152ae6-dde3-4bf5-8396-beb55f1f1232" />

### 🧠 Explanation

`SUM() OVER()` creates a window calculation without grouping the rows into one record.

Example:

```text
Order 101 → 150.50
Order 102 → 351.25
```

---

# 🏆 15. RANK() — Rank Orders

### 💡 Query Purpose
Rank orders according to their total amount.

```sql
SELECT
    OrderID,
    TotalAmount,
    RANK() OVER (
        ORDER BY TotalAmount DESC
    ) AS OrderRank
FROM Orders;
```
<img width="1069" height="99" alt="image" src="https://github.com/user-attachments/assets/53dacbd6-ce4d-4f5d-8de0-c5ea04b7bf72" />

### 🧠 Explanation

- Highest amount receives rank `1`.
- `RANK()` gives the same rank to tied values.
- Ranking is calculated from highest to lowest using `DESC`.

---

# 🏷️ 16. CASE — Assign Discount

### 💡 Query Purpose
Assign discounts according to order amount.

```sql
SELECT
    OrderID,
    TotalAmount,
    CASE
        WHEN TotalAmount > 1000 THEN '10% Discount'
        WHEN TotalAmount > 500 THEN '5% Discount'
        ELSE 'No Discount'
    END AS Discount
FROM Orders;
```
<img width="1035" height="104" alt="image" src="https://github.com/user-attachments/assets/c536ffa4-ea44-450f-92de-f11c97549a97" />

### 🧠 Explanation

`CASE` works like conditional logic:

```text
Amount > 1000  → 10% Discount
Amount > 500   → 5% Discount
Otherwise      → No Discount
```

---

# 👨‍💼 17. CASE — Employee Salary Category

### 💡 Query Purpose
Categorize employees according to salary.

> The exact salary boundaries were not specified in the project brief, so the thresholds below are an example and can be changed.

```sql
SELECT
    EmployeeID,
    FirstName,
    LastName,
    Salary,
    CASE
        WHEN Salary >= 70000 THEN 'High'
        WHEN Salary >= 50000 THEN 'Medium'
        ELSE 'Low'
    END AS SalaryCategory
FROM Employees;
```
<img width="885" height="109" alt="image" src="https://github.com/user-attachments/assets/44237242-fcc4-4198-9eff-fcd3878cac76" />

### 🧠 Explanation

The `CASE` expression assigns a category based on salary.

```text
₹70,000 or more → High
₹50,000–₹69,999 → Medium
Below ₹50,000   → Low
```

---

# 🛠️ Technologies Used

- 🐬 **MySQL**
- 🗄️ **SQL**
- ⚡ **XAMPP**
- 🖥️ **MySQL Workbench**
- 🌐 **phpMyAdmin**

---

# 📚 SQL Concepts Practiced

```text
🔗 JOINs
🔍 Subqueries
📅 Date Functions
🔤 String Functions
➕ Aggregate Functions
📊 Window Functions
🏷️ CASE Expressions
📈 Ranking
🧹 Data Cleaning
🔄 Data Transformation
```

---

# 🚀 Project Workflow

```text
       🗄️ Database
            │
            ▼
      📋 Create Tables
            │
            ▼
      📝 Insert Data
            │
            ▼
       🔗 JOIN Data
            │
            ▼
      🔍 Use Subqueries
            │
            ▼
       📅 Date Analysis
            │
            ▼
       🔤 String Cleanup
            │
            ▼
       📊 Aggregation
            │
            ▼
       📈 Window Functions
            │
            ▼
       🏷️ CASE Analysis
            │
            ▼
      📌 Final Results
```

---

# 🎓 Learning Outcomes

After completing this project, you should be able to:

- 🔗 Combine information from multiple tables.
- 🔍 Use subqueries for advanced filtering.
- 📅 Extract and format date information.
- 🔤 Manipulate strings using SQL functions.
- 📊 Calculate totals and averages.
- 📈 Create running totals and rankings.
- 🏷️ Apply conditional logic using `CASE`.
- 🧹 Clean and transform database data.
- 💼 Perform basic corporate data analysis using MySQL.

---

# 📁 Recommended Repository Structure

```text
Data-Transformer/
│
├── 📄 README.md
├── 📁 SQL/
│   ├── 01_database.sql
│   ├── 02_tables.sql
│   ├── 03_sample_data.sql
│   └── 04_queries.sql
│
└── 📁 Screenshots/
    └── query-results.png
```

---
<img width="945" height="805" alt="image" src="https://github.com/user-attachments/assets/805fcd7f-85d1-4690-a9f0-b57e4a3cc5d2" />

# ▶️ How to Run the Project

### 1️⃣ Start MySQL

Start **MySQL** using XAMPP or MySQL Workbench.

### 2️⃣ Create the Database

```sql
CREATE DATABASE data_transformer;
USE data_transformer;
```

### 3️⃣ Create the Tables

Create:

```text
Customers
Orders
Employees
```

### 4️⃣ Insert the Sample Data

Insert the sample records provided in this README.

### 5️⃣ Run the Queries

Execute the queries from **01 to 17** in MySQL.

---

# 🏅 Project Skills

| Category | Skills |
|---|---|
| 🗄️ Database | MySQL |
| 🔗 Relationships | PK / FK / JOIN |
| 🔍 Advanced SQL | Subqueries |
| 📅 Date Analysis | YEAR, MONTH, DATEDIFF, DATE_FORMAT |
| 🔤 Text Processing | CONCAT, REPLACE, UPPER, LOWER, TRIM |
| 📊 Analytics | SUM, AVG |
| 📈 Window Functions | RANK, Running Total |
| 🧠 Logic | CASE |
| 🧹 Transformation | Data Cleaning & Formatting |

---

## 📌 Project Information

**Project Name:** Data Transformer  
**Project Type:** SQL / Database Project  
**Database:** MySQL  
**Focus:** Data Transformation & Analysis  
**Query Count:** 17  

> 💬 **“Quality is our Motto.”**

---
👨‍💻 Author
Name: Kush Kumar

### ⭐ If this project helps you practice SQL, consider giving the repository a star!
