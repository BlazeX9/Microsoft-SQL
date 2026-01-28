### SQL Joining
- **Self Join**: It's used when a table needs to be joined with itself to compare rows within the same table.<br>
  `SELECT a.ID,a.EName,a.ESalary FROM Employee a JOIN Employee b ON a.ESalary=b.ESalary`
- **Cross Join**: It returns every possible combination of rows from both tables. It does not use a join condition and the total number of rows in the result is the number of rows in the first table multiplied by the number of rows in the second table.

- **Inner Join**: It returns only the rows that have matching values in both tables, filtering out non-matching records.

- **Outer Join**: Outer Joins include rows that do not have a corresponding match in one or both of the tables.<br>
  `LEFT OUTER JOIN` `RIGHT OUTER JOIN` `FULL OUTER JOIN`


### Duplicate Data
- **Identifying duplicate data**: `SELECT column, COUNT(*) FROM table_name GROUP BY column HAVING COUNT(*) > 1`
- **Viewing all duplicate data**: `SELECT *, COUNT(*) OVER (PARTITION BY column) AS duplicate_count FROM table_name`
- **Deleting all dublicate data**: `WITH CTE AS (SELECT *,ROW_NUMBER() OVER (PARTITION BY column1,column2 ORDER BY column3) AS rn FROM employees)`<br>`DELETE FROM employees WHERE ID IN (SELECT ID FROM CTE WHERE rn > 1)`
