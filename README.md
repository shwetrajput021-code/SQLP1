DATADIGGER – MySQL Practice Database

A small e-commerce database built in MySQL to practice core SQL: creating tables, inserting data, CRUD operations, filtering, aggregate functions, foreign keys and joins.

Database Structure
Customers (1) ────< Orders (1) ────< OrderDetails >──── (1) Products
Table	Purpose	Columns
Customers	Stores customer info	CustomerID (PK, auto-increment), Name, Email, Address
Orders	One row per order placed	OrderID (PK, auto-increment), CustomerID (FK → Customers), OrderDate, TotalAmount
Products	Product catalog	ProductID (PK, auto-increment), ProductName, Price, Stock (default 0)
OrderDetails	Line items of each order	OrderDetailID (PK, auto-increment), OrderID, ProductID, Quantity, SubTotal

Orders.CustomerID has ON DELETE CASCADE, so deleting a customer also deletes their orders.

Getting Started

Requirements: MySQL 5.7+ / 8.x (or MariaDB) and any client (MySQL Workbench, CLI, DBeaver).

bash
mysql -u root -p < datadigger.sql

Or paste the script into MySQL Workbench and run it. Tables must be created in this order: Customers → Orders → Products → OrderDetails.

sql
CREATE DATABASE DATADIGGER;
USE DATADIGGER;
What the Script Covers
1. Customers
INSERT 5 customers (Alice, Rahul, Priya, John, Krish)
SELECT * to view all customers
UPDATE Alice's address to Gandhinagar
DELETE customer with CustomerID = 5 (Krish)
SELECT ... WHERE Name = 'Alice'
2. Orders
Foreign key to Customers with cascade delete
INSERT 5 orders
Fetch all orders of customer 1
UPDATE order 1 total to 1800.00
DELETE order 5
Filter orders with OrderDate >= '2026-10-05'
Aggregates: MAX, MIN, AVG of TotalAmount
3. Products
INSERT 5 products (Laptop, Mouse, Keyboard, Headphones, Old USB Cable)
Sort by price descending (ORDER BY Price DESC)
UPDATE Mouse price to 650.00
DELETE out-of-stock products (Stock = 0)
Range filter with BETWEEN 500 AND 2000
Most expensive and cheapest product
4. OrderDetails
INSERT 5 line items
Items in a specific order (OrderID = 1)
Total revenue with SUM(SubTotal)
Top 3 most ordered products using INNER JOIN, GROUP BY, ORDER BY and LIMIT
How many times a product was sold (COUNT(*))
Sample Results (after all changes in the script)
Query	Result
Orders since 2026-10-05	Order IDs 1, 2, 3
Highest / Lowest / Average order	3200.00 / 800.00 / 2075.00
Products priced 500–2000	Mouse (650), Keyboard (1200), Headphones (1800)
Most expensive / Cheapest product	45000.00 / 650.00
Total revenue	7350.00
Top 3 ordered products	Mouse (3), Keyboard (3), Headphones (1)
Times Mouse (ProductID 2) was sold	2
Key Concepts Practiced
CREATE DATABASE / CREATE TABLE
Constraints: PRIMARY KEY, AUTO_INCREMENT, NOT NULL, DEFAULT, FOREIGN KEY
CRUD: INSERT, SELECT, UPDATE, DELETE
Filtering: WHERE, BETWEEN, comparison operators
Sorting and limiting: ORDER BY, LIMIT
Aggregates: MAX, MIN, AVG, SUM, COUNT
Joins and grouping: INNER JOIN, GROUP BY
Notes
Plain-text headings and the line "Orders table banao…" in the script must be turned into SQL comments (-- ...) or removed, otherwise MySQL will throw a syntax error.
OrderDetails has no foreign keys on OrderID and ProductID. Adding them would enforce data integrity:
sql
  ALTER TABLE OrderDetails
    ADD FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    ADD FOREIGN KEY (ProductID) REFERENCES Products(ProductID);
DELETE FROM Products WHERE ProductID > 0 AND Stock = 0; includes ProductID > 0 so it works with MySQL Workbench's Safe Updates mode.
Order TotalAmount values are sample data and don't necessarily match the sum of their OrderDetails rows.
Author

Created as a hands-on SQL practice project.
