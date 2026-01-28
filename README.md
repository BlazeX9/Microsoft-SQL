### SQL Joining
- Self Join: It's used when a table needs to be joined with itself to compare rows within the same table. 
              `SELECT a.ID,a.EName,a.ESalary FROM Employee a JOIN Employee b ON a.ESalary=b.ESalary`
- Cross Join: It returns every possible combination of rows from both tables. It does not use a join condition and the total number of rows in the result is the number of rows in the first table multiplied by the number of rows in the second table.

- Inner Join: 
