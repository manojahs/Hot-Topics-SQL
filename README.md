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

After 2NF
| EmpID | EmpName | ProjectName | ProjectManager |
| ----- | ------- | ----------- | -------------- |
| 1     | Manoj   | ERP System  | Raj            |
| 1     | Manoj   | Payroll     | Anita          |
| 2     | Priya   | Recruitment | Neha           |
| 3     | Rahul   | ERP System  | Raj            |


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

Formula to Identify Transitive Dependency

If A → B and B → C, then C is transitively dependent on A.
To satisfy 3NF: Remove C to a separate table so it depends directly on its own key (B), not indirectly on A.

Here employeeiD depend on project and project dependent on Projectmanager so indirelcty employeeid dependent on projectmanager so in that case EMpID is primary key and Project and Projectmanager are non primary key so we need to create seperate table for that

ProjectManager depends only on ProjectName (the key of Project table), not on EmpID.
No non-key attribute depends on another non-key attribute in the same table.
All dependencies are direct from key → non-key.


| Normal Form | What it Removes       | Example                                    |
| ----------- | --------------------- | ------------------------------------------ |
| 1NF         | Repeating groups      | Split Project1, Project2 columns into rows |
| 2NF         | Partial dependency    | Move Dept info to separate table           |
| 3NF         | Transitive dependency | Move Project Manager to Project table      |

| **Property**        | **Meaning**     | **Explanation**                                                                 | **Example (SQL)**                                                                          |
| ------------------- | --------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| **A - Atomicity**   | All or nothing  | A transaction is either fully completed or not at all. No partial transactions. | If you transfer money from Account A to B, both debit and credit must happen — or neither. |
| **C - Consistency** | Valid state     | Database remains in a valid state before and after the transaction.             | If a balance can't go negative, a transaction violating this won't be allowed.             |
| **I - Isolation**   | No interference | Concurrent transactions do not interfere with each other.                       | Two users transferring funds simultaneously won't corrupt the data.                        |
| **D - Durability**  | Permanent       | Once committed, changes persist even if the system crashes.                     | After a `COMMIT`, the data remains stored permanently.                                     |

Atomicity
-----------
BEGIN TRANSACTION;

-- Atomicity: Both steps must succeed
UPDATE accounts SET balance = balance - 500 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 500 WHERE account_id = 2;

-- Commit ensures Durability
COMMIT;

Consistency
---------

CREATE TABLE accounts (
    account_id INT PRIMARY KEY,
    name VARCHAR(50),
    balance DECIMAL(10,2) CHECK (balance >= 0)
);

BEGIN TRANSACTION;

UPDATE accounts
SET balance = balance - 1000
WHERE account_id = 1;

COMMIT;

How to improve the Performance of Stored Proc
------------------------------------------------



Quick Checklist for SP Optimization

✅ Set NOCOUNT ON

✅ Only select needed columns

✅ Index WHERE, JOIN, ORDER BY columns

✅ Filter early

✅ Avoid cursors, prefer set-based operations

✅ Batch large DML operations

✅ Avoid functions on indexed columns

✅ Consider parameter sniffing



```


