### SQL Joining
- Self Join: It's used when a table needs to be joined with itself to compare rows within the same table. 
              `SELECT a.ID,a.EName,a.ESalary FROM Employee a JOIN Employee b ON a.ESalary=b.ESalary`
- Cross Join: It returns every possible combination of rows from both tables. It does not use a join condition and the total number of rows in the result is the number of rows in the first table multiplied by the number of rows in the second table.

- Inner Join: It returns only the rows that have matching values in both tables, filtering out non-matching records.

- Outer Join: Outer Joins include rows that do not have a corresponding match in one or both of the tables.
  `LEFT OUTER JOIN` `RIGHT OUTER JOIN` `FULL OUTER JOIN`
