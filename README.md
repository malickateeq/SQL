# Databases, Queries and Relationships

## Introduction
- SQL is Structured Query Langauage while mySQL (DBMS) is a Software which uses SQL language to manage databases
- SQL keywords are NOT case sensitive: select is the same as SELECT.
- Some of The Most Important SQL Commands:
    * SELECT - extracts data from a database
    * UPDATE - updates data in a database
    * DELETE - deletes data from a database
    * INSERT INTO - inserts new data into a database
    * CREATE DATABASE - creates a new database
    * ALTER DATABASE - modifies a database
    * CREATE TABLE - creates a new table
    * ALTER TABLE - modifies a table
    * DROP TABLE - deletes a table
    * CREATE INDEX - creates an index (search key)
    * DROP INDEX - deletes an index

## The SQL SELECT Statement

```sql
SELECT column1, column2, ...
FROM table_name;
-- OR
SELECT * FROM table_name;
```

### The SQL SELECT DISTINCT Statement
```sql
-- used to return only distinct (different) values. 
-- will SKIP duplicate values

SELECT DISTINCT column1, column2, ...
FROM table_name;

-- The following SQL statement lists (return) the number of different (distinct) customer countries: a number eg; 10
SELECT COUNT(DISTINCT Country) FROM Customers;
-- OR
SELECT Count(*) AS DistinctCountries
FROM (SELECT DISTINCT Country FROM Customers);
```
## The WHERE condition in SQL
```sql
SELECT * FROM Customers
WHERE Country='Mexico';
```
- Operators in WHERE clause
```sql
=   	Equal	
>	    Greater than	
<	    Less than	
>=	    Greater than or equal	
<=	    Less than or equal	
<>	    Not equal. Note: In some versions of SQL this operator may be written as !=	
BETWEEN	Between a certain range	
LIKE	Search for a pattern	
IN	    To specify multiple possible values for a column
``` 
## The SQL AND, OR and NOT Operators
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition1 AND condition2 AND condition3 ...;

SELECT column1, column2, ...
FROM table_name
WHERE NOT condition;
-- OR
SELECT * FROM Customers
WHERE Country!='Germany';

-- Combining AND, OR and NOT; For complex expressions
SELECT * FROM Customers
WHERE Country='Germany' AND (City='Berlin' OR City='München');

```
## SQL ORDER BY Keyword
```sql
-- The ORDER BY keyword is used to sort the result-set in ascending or descending order.
-- sorts the records in ascending order by default.
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;

-- This means that it orders by Country, but if some rows have the same Country, it orders them by CustomerName
SELECT * FROM Customers
ORDER BY Country, CustomerName;

-- Order different columns
SELECT * FROM Customers
ORDER BY Country ASC, CustomerName DESC;
```

## The SQL INSERT INTO Statement
```sql
-- 1st method: Specify Both
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);

-- 2nd Method: adding all values. same order as in DB
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

## SQL NULL Values
```sql
-- To check whether is null or not
SELECT column_names
FROM table_name
WHERE column_name IS NULL;

SELECT column_names
FROM table_name
WHERE column_name IS NOT NULL;
```

## The SQL UPDATE Statement
```sql
-- Update record(s), 
-- If you omit the WHERE clause, ALL records will be updated!
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;

UPDATE Customers
SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
WHERE CustomerID = 1;
```

## The DELETE Statement
```sql
-- If you omit the WHERE clause, all records in the table will be deleted!
DELETE FROM table_name WHERE condition;

-- DELETE all records
DELETE FROM table_name; 
```

## SQL TOP(SQL Server,MS Access), LIMIT(mySQL) or ROWNUM(Oracle) Clause
- used to specify the number of records to return.
```sql
SELECT TOP number|percent column_name(s)
FROM table_name
WHERE condition;

-- also
SELECT column_name(s)
FROM table_name
WHERE condition
LIMIT number;

SELECT * FROM Customers
LIMIT 3;
```

## SQL MIN() and MAX() Functions
- The MIN() function returns the smallest value of the selected column.
- The MAX() function returns the largest value of the selected column.
```sql
SELECT MIN(column_name)
FROM table_name
WHERE condition;

SELECT MIN(Price) AS SmallestPrice
FROM Products; 
-- Result: SmallestPrice 120
```

## SQL COUNT(), AVG() and SUM() Functions
- The COUNT() function returns the number of rows that matches a specified criteria.
- The AVG() function returns the average value of a numeric column.
- The SUM() function returns the total sum of a numeric column.

```sql
SELECT COUNT(column_name)
FROM table_name
WHERE condition;

SELECT AVG(column_name)
FROM table_name
WHERE condition;

SELECT SUM(column_name)
FROM table_name
WHERE condition;
```

## SQL LIKE Operator
- to search for a specified pattern in a column.

* % - The percent sign represents zero, one, or multiple characters
* _ - The underscore represents a single character

```sql
SELECT column1, column2, ...
FROM table_name
WHERE columnN LIKE pattern;

    LIKE Operator:	                     Description:
WHERE CustomerName LIKE 'a%'	        values that start with "a"
WHERE CustomerName LIKE '%a'	        values that end with "a"
WHERE CustomerName LIKE '%or%'	        values that have "or" in any position
WHERE CustomerName LIKE '_r%'	        values that have "r" in the second position
WHERE CustomerName LIKE 'a__%'	        values that start with "a" and are at least 3 characters in length
WHERE ContactName LIKE 'a%o'	        values that start with "a" and ends with "o"
```

## SQL Wildcards
- used to substitute one or more characters in a string
```sql
SELECT * FROM Customers
WHERE City LIKE '[bsp]%';

Symbol	    Description	                            Example
*	     zero or more characters	        bl* finds bl, black, blue, and blob
?	     a single character	                h?t finds hot, hat, and hit
[]	     any single character within it 	h[oa]t finds hot and hat, but not hit
!	     any character not in the brackets	h[!oa]t finds hit, but not hot and hat
-	     a range of characters	            c[a-b]t finds cat and cbt
#	     any single numeric character	    2#5 finds 205, 215, 225, 235, 245, 255, 265, 275, 285, and 295
```

## SQL IN Operator
- The IN operator allows you to specify multiple values in a WHERE clause.
- The IN operator is a shorthand for multiple OR conditions.
```sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1, value2, ...);

-- OR
SELECT column_name(s)
FROM table_name
WHERE column_name IN (SELECT STATEMENT);

SELECT * FROM Customers
WHERE Country IN ('Germany', 'France', 'UK');

SELECT * FROM Customers
WHERE Country NOT IN ('Germany', 'France', 'UK');

SELECT * FROM Customers
WHERE Country IN (SELECT Country FROM Suppliers);
```

## SQL BETWEEN Operator
- The BETWEEN operator selects values within a given range. The values can be numbers, text, or dates.
- The BETWEEN operator is inclusive: begin and end values are included. 
```sql
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;

SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20;

SELECT * FROM Products
WHERE Price NOT BETWEEN 10 AND 20;
```

## SQL Aliases
- SQL aliases are used to give a table, or a column in a table, a temporary name.
```sql
-- Alias Column Syntax
SELECT column_name AS alias_name
FROM table_name;
-- Alias Table Syntax
SELECT column_name(s)
FROM table_name AS alias_name;

SELECT CustomerName AS Customer, ContactName AS 'Contact Person'    -- if Alias contains spaces
FROM Customers;

-- Return in one Address combines
SELECT CustomerName, Address + ', ' + PostalCode + ' ' + City + ', ' + Country AS Address
FROM Customers;
-- or
SELECT CustomerName, CONCAT(Address,', ',PostalCode,', ',City,', ',Country) AS Address
FROM Customers;

-- 
SELECT o.OrderID, o.OrderDate, c.CustomerName
FROM Customers AS c, Orders AS o
WHERE c.CustomerName="Around the Horn" AND c.CustomerID=o.CustomerID;
-- or
SELECT Orders.OrderID, Orders.OrderDate, Customers.CustomerName
FROM Customers, Orders
WHERE Customers.CustomerName="Around the Horn" AND Customers.CustomerID=Orders.CustomerID;
```

## SQL Joins

- A JOIN clause is used to combine rows from two or more tables, based on a related column between them.
```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
```

### Something About Joins
![Image of Yaktocat](/img/joins.png)

### SQL INNER JOIN Keyword
- The INNER JOIN keyword selects records that have matching values in both tables.
```sql
-- syntax
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;

SELECT Orders.OrderID, Customers.CustomerName
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

### SQL LEFT JOIN Keyword
- The LEFT JOIN keyword returns all records from the left table (table1), and the matched records from the right table (table2). The result is NULL from the right side, if there is no match.

```sql
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;

SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
LEFT JOIN Orders ON Customers.CustomerID = Orders.CustomerID
ORDER BY Customers.CustomerName;
```

### SQL RIGHT JOIN Keyword
- The RIGHT JOIN keyword returns all records from the right table (table2), and the matched records from the left table (table1). The result is NULL from the left side, when there is no match.

```sql
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name;

SELECT Orders.OrderID, Employees.LastName, Employees.FirstName
FROM Orders
RIGHT JOIN Employees ON Orders.EmployeeID = Employees.EmployeeID
ORDER BY Orders.OrderID;
```

### SQL FULL OUTER JOIN Keyword
- The FULL OUTER JOIN keyword returns all records when there is a match in left (table1) or right (table2) table records.

```sql
SELECT column_name(s)
FROM table1
FULL OUTER JOIN table2
ON table1.column_name = table2.column_name
WHERE condition;

SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName;
```

### SQL Self JOIN
- A self JOIN is a regular join, but the table is joined with itself.
```sql
SELECT column_name(s)
FROM table1 T1, table1 T2
WHERE condition;

-- The following SQL statement matches customers that are from the same city:
SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City
ORDER BY A.City;
```

### The SQL UNION Operator
- The UNION operator is used to combine the result-set of two or more SELECT statements.
    * Each SELECT statement within UNION must have the same number of columns
    * The columns must also have similar data types
    * The columns in each SELECT statement must also be in the same order

```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;

SELECT City FROM Customers
UNION   -- use UNION ALL to allow DISTINCT values
SELECT City FROM Suppliers
ORDER BY City;
```

## The SQL GROUP BY Statement
- The GROUP BY statement groups rows that have the same values into summary rows, like "find the number of customers in each country".

- The GROUP BY statement is often used with aggregate functions (COUNT, MAX, MIN, SUM, AVG) to group the result-set by one or more columns.

```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
```

## The SQL HAVING Clause
- The HAVING clause was added to SQL because the WHERE keyword could not be used with aggregate functions.
- To put condition on Aggregate functions.
```sql
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);

SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;
```

## The SQL EXISTS Operator
- The EXISTS operator is used to test for the existence of any record in a subquery.
- The EXISTS operator returns true if the subquery returns one or more records.
```sql
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);

SELECT SupplierName
FROM Suppliers
WHERE EXISTS (SELECT ProductName FROM Products WHERE Products.SupplierID = Suppliers.supplierID AND Price < 20);
```

## The SQL ANY and ALL Operators
- The ANY and ALL operators are used with a WHERE or HAVING clause.
- The ANY operator returns true if any of the subquery values meet the condition.
- The ALL operator returns true if all of the subquery values meet the condition.
```sql
SELECT column_name(s)
FROM table_name
WHERE column_name operator ANY
(SELECT column_name FROM table_name WHERE condition);

SELECT ProductName
FROM Products
WHERE ProductID = ANY (SELECT ProductID FROM OrderDetails WHERE Quantity = 10);
```

# SQL Database

## The SQL CREATE DATABASE Statement
```sql
-- Create a new database
CREATE DATABASE databasename;

-- DROP/Delete Database
DROP DATABASE databasename;

-- BackUp Database
BACKUP DATABASE databasename
TO DISK = 'filepath';

-- Create Tabel
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);

CREATE TABLE Persons (
    PersonID int,
    LastName varchar(255),
    FirstName varchar(255),
    Address varchar(255),
    City varchar(255)
)

-- Drop/Delete Table
DROP TABLE table_name;

-- Alter Table
ALTER TABLE table_name
ADD column_name datatype;

ALTER TABLE Customers
ADD Email varchar(255);
```
