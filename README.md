# Hot-Topics-SQL


<img width="400" alt="image" src="https://github.com/user-attachments/assets/4b32ac6b-33c8-42d5-b484-0cad9da928cc" />

<img width="655" alt="image" src="https://github.com/user-attachments/assets/5785260d-4531-4803-b540-ea5875179674" />


```
Find the 2nd highest salary 
-------------------------
Using DenseRank()
----------------

SELECT salary
FROM (
  SELECT salary,
         DENSE_RANK() OVER (ORDER BY salary DESC) AS dr
  FROM Employees
) t
WHERE dr = 2;

Delete the duplicate records 
-----------------
WITH Ranked AS (
  SELECT
    PK_ID,
    ROW_NUMBER() OVER (
      PARTITION BY ColA, ColB
      ORDER BY PK_ID
    ) AS rn
  FROM MyTable
)
DELETE FROM Ranked
WHERE rn > 1;

OR
2nd highest salary
-----------------------

select  salary from (
select Distinct salary ,Dense_rank() over (order by salary desc) as T
from EMployees
) as Salarydate
where T=2

OR


;with SalaryData as
(
select salary,DENSE_RANK() over (order by salary desc) as T
from EMployees
)

select Distinct Salary from SalaryData where T = 2

2nd highest salary using dept wise
-----------------------------------


select Salary,deptname from
(
select distinct salary,deptname, DENSE_RANK() over (partition by deptname order by salary desc) as T
from EMployee
) as data
where T=2 

Normalization
-----------------
Normalization is a database design technique that organizes tables and their relationships to:

Reduce data redundancy (duplicate data)
Improve data integrity
Make the database more efficient and easier to maintain
It involves breaking down a table into smaller tables and defining relationships between them.

1NF
---
first step of normalization is to convert these repeating columns into multiple rows, making the table truly in 1NF.

No repeating groups
Each cell has atomic value
Each column represents one attribute

| EmpID | EmpName | DepartmentName | DepartmentLocation | Project1    | Project2 |
| ----- | ------- | -------------- | ------------------ | ----------- | -------- |
| 1     | Manoj   | IT             | Bangalore          | ERP System  | Payroll  |
| 2     | Priya   | HR             | Mumbai             | Recruitment | NULL     |
| 3     | Rahul   | IT             | Bangalore          | ERP System  | NULL     |

In this example it has atomic columns but Project1 , Project2 are same type of information
so we need to make it as single group column for example Project


| EmpID | EmpName | DepartmentName | DepartmentLocation | Project     |
| ----- | ------- | -------------- | ------------------ | ----------- |
| 1     | Manoj   | IT             | Bangalore          | ERP System  |
| 1     | Manoj   | IT             | Bangalore          | Payroll     |
| 2     | Priya   | HR             | Mumbai             | Recruitment |
| 3     | Rahul   | IT             | Bangalore          | ERP System  |

2NF
------
Rule: Remove partial dependency.

Department details depend only on DepartmentName, not on (EmpID + Project).
So separate department details into their own table.

Employee Table

| EmpID | EmpName | DepartmentName |
| ----- | ------- | -------------- |
| 1     | Manoj   | IT             |
| 2     | Priya   | HR             |
| 3     | Rahul   | IT             |

Department Table

| DepartmentName | DepartmentLocation |
| -------------- | ------------------ |
| IT             | Bangalore          |
| HR             | Mumbai             |

ProjectAssign Table

| EmpID | Project     |
| ----- | ----------- |
| 1     | ERP System  |
| 1     | Payroll     |
| 2     | Recruitment |
| 3     | ERP System  |

3NF
-------------
Rule: Remove transitive dependency.
Suppose we also store Project Manager, which depends on Project, not EmpID. So we split Project details too.

Project Table

| ProjectName | ProjectManager |
| ----------- | -------------- |
| ERP System  | Raj            |
| Payroll     | Anita          |
| Recruitment | Neha           |

📝 ProjectAssignment Table (links Employee ↔ Project)

| EmpID | ProjectName |
| ----- | ----------- |
| 1     | ERP System  |
| 1     | Payroll     |
| 2     | Recruitment |
| 3     | ERP System  |

Final Normalized Structure

Employee Table — stores employee info
Department Table — stores department info
Project Table — stores project info
ProjectAssignment Table — links employees to projects (many-to-many)

| Normal Form | What it Removes       | Example                                    |
| ----------- | --------------------- | ------------------------------------------ |
| 1NF         | Repeating groups      | Split Project1, Project2 columns into rows |
| 2NF         | Partial dependency    | Move Dept info to separate table           |
| 3NF         | Transitive dependency | Move Project Manager to Project table      |


```


