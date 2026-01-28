### Data Merging
- **UNION**: Returns only **unique** rows from the selected tables.
- **UNION ALL**: Returns **all** rows from the selected tables.
- **INTERSECT**: Returns only **common** rows from the selected tables.
- **EXCEPT**: Returns all rows from left side table which are not present in right side table.

Number of columns, order of the columns and datatype of the columns needs to be same on both select statement when using above operators.<br>
`SELECT column1,column2 FROM table1 UNION SELECT column1,column2 FROM table2`<br><br><br>

### SQL Joining
- **Self Join**: It's used when a table needs to be joined with itself to compare rows within the same table.<br>
  `SELECT a.Emp_ID,a.Emp_Name,a.Emp_Salary FROM Employee a JOIN Employee b ON a.Emp_Salary=b.Emp_Salary`
- **Cross Join**: It returns every possible combination of rows from both tables. It does not use a join condition and the total number of rows in the result is the number of rows in the first table multiplied by the number of rows in the second table.<br>
  `SELECT * FROM table1 CROSS JOIN table2`

- **Inner Join**: It returns only the rows that have matching values in both tables, filtering out non-matching records.<br>
  `SELECT columns FROM table1 INNER JOIN table2 ON table1.column_name = table2.column_name`

- **Outer Join**: Outer Joins include rows that do not have a corresponding match in one or both of the tables.<br>
  `LEFT OUTER JOIN` `RIGHT OUTER JOIN` `FULL OUTER JOIN`<br><br><br>

### Duplicate Data Handeling
- **Identifying duplicate data**: `SELECT Emp_Name,Emp_Department,COUNT(*) AS duplicate_count FROM Employee GROUP BY Emp_Name,Emp_Department HAVING COUNT(*) > 1`
- **Deleting duplicate data**: `WITH CTE AS (SELECT *,ROW_NUMBER() OVER (PARTITION BY Emp_Name,Emp_Department ORDER BY Emp_ID) AS rn FROM employees)`
   `DELETE FROM employees WHERE Emp_ID IN (SELECT Emp_ID FROM CTE WHERE rn > 1)`
- **Viewing only unique records**: `DISTINCT`<br><br><br>

### CTE
Common Table Expression is a temporary result set in SQL<br><br>

### Pattern Matching | LIKE Operator
- 'a%' - Match strings that start with 'a'
- '%a' - Match strings with end with 'a'
- 'a%t' - Match strings that contain the start with 'a' and end with 't'
- '%wow%' - Match strings that contain the substring 'wow' in them at any position
- '_a%' - Match strings that contain 'a' at the second position
- 'a_ _%' - Match strings that start with 'a and contain at least 2 more characters

### Functions
- **ROW_NUMBER()**: Same ranking value is shown for dublicate row values. Ex: two people having same salary will have same rank<br>
  `SELECT Emp_Name,Emp_Salary,ROW_NUMBER() OVER(PARTITION BY Emp_Department ORDER BY Emp_Salary DESC) AS Rank FROM Employee`
- **RANK()**: Same ranking value is shown for dublicate row values and skips next rank
- **DENSE_RANK()**: Overcomes the issues of ROW_NUMBER and RANK
- **IDENTITY()**: Auto increments value<br>
  `CREATE TABLE Employee (Emp_ID INT IDENTITY(1001,1))`
- **GETDATE()**: Returns the system current date and time<br><br><br>

### Queries
1. Finding the second highest salary person<br> 
```
SELECT EmpName,EmpSalary 
FROM (SELECT EmpName,EmpSalary,DENSE_RANK() OVER (ORDER BY EmpSalary DESC) AS Ranks FROM Employee) t 
WHERE Ranks = 2
```
```
WITH CTE AS (SELECT EmpName,EmpSalary,DENSE_RANK() OVER (ORDER BY EmpSalary DESC) AS Ranks FROM Employee)   
SELECT EmpName,EmpSalary FROM CTE WHERE Ranks = 2
```
<br><br>

### View
View is a virtual table created from the result of a SELECT query. It does not store data physically. Views help in enhance security and present data in a cleaner, customized format.

- **Simple View**: When view is created on a single table, supports all DML Operations.
```
CREATE VIEW simpleView AS
SELECT NAME, ADDRESS FROM employee;

SELECT * FROM simpleView
```
- **Complex View**: When view is create on multiple tables, does not supports all DML operations.
```
CREATE VIEW complexView AS
SELECT a.empid,a.empname,b.empsalary,b.empcity
FROM emp_table_1 a JOIN emp_table_2 b ON a.empid=b.empid;
	
SELECT * FROM complexView
```
- **Listing all Views in a Database**: 
```
SHOW FULL TABLES WHERE table_type LIKE "%VIEW"
```
- **Deleting a View**:
```
DROP VIEW view_name
```
<br>

### Stored Procedures
Stored procedures are precompiled collection of SQL statements bundled together to perform a specific task. These procedures are stored in the database and can be called upon by users, applications or other procedures. Stored procedures are essential for automating database tasks, improving efficiency and reducing redundancy.

- **System Stored Procedures**:<br>
	`sp_help` `sp_rename`
- **User Defined**:
```
CREATE PROCEDURE GetCustomersByCountry AS
@Country VARCHAR(50)
BEGIN 
SELECT CustomerName,ContactName FROM Customers WHERE Country = @Country;
END;

EXECUTE GetCustomersByCountry @Country = 'Sri lanka';
```
**Advantages of Stored Procedures**
- Since stored procedures are precompiled they execute faster than running ad-hoc SQL queries.
- Stored procedures can be reused in multiple applications or different parts of an application. This reduces the need to rewrite queries repeatedly.
- Instead of sending multiple individual queries to the database server stored procedures allow to execute multiple operations in one go, reducing network load.
- Stored procedures simplify code maintenance. Changes made to the procedure are automatically reflected wherever the procedure is used.<br><br>


### Stored Function
A stored function is a set of SQL statements that perform some operation and return a single value.

- **Scalar Valued Function**: This function takes one or more parameters and returns a single value
```
CREATE FUNCTION MyStoredFunc(@EID INT) 
RETURNS MONEY AS 
BEGIN 
DECLARE @BASIC MONEY,@HRA MONEY,@DA MONEY,@GROSS MONEY 
SELECT @BASIC=ESalary FROM Employee WHERE EID=@EID 
SET @HRA=@BASIC*0.1 
SET @DA=@BASIC*0.1 
SET @GROSS=@BASIC+@HR+@DA 
RETURN @GROSS 
END

SELECT dbo.MyStoredProcedure(E1001)
```

- **Table Valued Functions**: This function returns more than one value
```
CREATE FUNCTION MyStoredFunc(@EDept VARCHAR(50)) 
RETURNS TABLE AS 
RETURN (SELECT * FROM Employee WHERE Department=@EDept)

SELECT * FROM MyStoredFunc('Admin')
```

### Stored Procedure vs Stored Function
- Stored procedure is precomplied and executed only whenever it is called but stored function is complied and executed everytime it is called
- Stored function must return a value but in a stored procedure it is optional
- Stored function can be called from stored procedure but vice versa is not possible
- DML(Insert/Update/Delete) operations are not possible in stored function, it only allows SELECT statememnt in it


### Indexing
Indexes are special database structures that speed up data retrieval by allowing quick access to records instead of scanning the entire table. They act like a lookup system and play an important role in improving query performance and database efficiency.

**Clustered Index**: Sorts and stores data rows in the table based on the index key<br>
**Non-clustered Index**: Creates a separate structure from the data rows to store index keys and pointers

```
CREATE INDEX idx_product ON Sales (product_id)
```
```
CREATE UNIQUE INDEX idx_unique_employee ON Sales (customer_id)
```

**Viewing Indexes**
```
SHOW INDEXES FROM table_name
```
**Removing an Index**
```
DROP INDEX index_name
```


### Performance Tuning
SQL Server Management Studio allows users to view the execution plan which details how SQL Server processes a query. This plan helps identify inefficiencies like missing indexes or unnecessary table scans.

1. Avoide SELECT *
2. Avoid SELECT DISTINCT
3. Use INNER JOIN Instead of WHERE for Joins
3. Use WHERE Instead of HAVING
4. Use LIMIT
