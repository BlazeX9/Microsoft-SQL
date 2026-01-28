### Data Merging
- **UNION**: Returns only **unique** rows from the selected tables.
- **UNION ALL**: Returns **all** rows from the selected tables.
- **INTERSECT**: Returns only **common** rows from the selected tables.
- **EXCEPT**: Returns all rows from left side table which are not present in right side table.

Number of columns, order of the columns and datatype of the columns needs to be same on both select statement when using above operators.<br>
`SELECT column1,column2 FROM table1 UNION SELECT column1,column2 FROM table2`


### SQL Joining
- **Self Join**: It's used when a table needs to be joined with itself to compare rows within the same table.<br>
  `SELECT a.Emp_ID,a.Emp_Name,a.Emp_Salary FROM Employee a JOIN Employee b ON a.Emp_Salary=b.Emp_Salary`
- **Cross Join**: It returns every possible combination of rows from both tables. It does not use a join condition and the total number of rows in the result is the number of rows in the first table multiplied by the number of rows in the second table.<br>
  `SELECT * FROM table1 CROSS JOIN table2`

- **Inner Join**: It returns only the rows that have matching values in both tables, filtering out non-matching records.<br>
  `SELECT columns FROM table1 INNER JOIN table2 ON table1.column_name = table2.column_name`

- **Outer Join**: Outer Joins include rows that do not have a corresponding match in one or both of the tables.<br>
  `LEFT OUTER JOIN` `RIGHT OUTER JOIN` `FULL OUTER JOIN`


### Duplicate Data Handeling
- **Identifying duplicate data**: `SELECT Emp_Name,Emp_Department,COUNT(*) AS duplicate_count FROM Employee GROUP BY Emp_Name,Emp_Department HAVING COUNT(*) > 1`
- **Viewing duplicate data**: `SELECT *, COUNT(*) OVER (PARTITION BY Emp_Name,Emp_Department) AS duplicate_count FROM Employee`
- **Deleting duplicate data**: `WITH CTE AS (SELECT *,ROW_NUMBER() OVER (PARTITION BY Emp_Name,Emp_Department ORDER BY Emp_ID) AS rn FROM employees) DELETE FROM employees WHERE Emp_ID IN (SELECT Emp_ID FROM CTE WHERE rn > 1)`
- **Viewing only unique records**: `DISTINCT`


### CTE
Common Table Expression is a temporary result set in SQL


### Functions
- **ROW_NUMBER()**: Same ranking value is shown for dublicate row values. Ex: two people having same salary will have same rank<br>
  `SELECT Emp_Name,Emp_Salary,ROW_NUMBER() OVER(PARTITION BY Emp_Department ORDER BY Emp_Salary DESC) AS Rank FROM Employee`
- **RANK()**: Same ranking value is shown for dublicate row values and skips next rank
- **DENSE_RANK()**: Overcomes the issues of ROW_NUMBER and RANK
- **IDENTITY()**: Auto increments value<br>
  `CREATE TABLE Employee (Emp_ID INT IDENTITY(1001,1))`
- **GETDATE()**: Returns the system current date and time


### Queries
1. Finding the second highest salary person<br> 
`SELECT EmpName,EmpSalary FROM (SELECT EmpName,EmpSalary,DENSE_RANK() OVER (ORDER BY EmpSalary DESC) AS Ranks FROM Employee) t WHERE Ranks = 2`<br>
```
WITH CTE AS (SELECT EmpName,EmpSalary,DENSE_RANK() OVER (ORDER BY EmpSalary DESC) AS Ranks FROM Employee) 
SELECT EmpName,EmpSalary FROM CTE WHERE Ranks = 2
```
