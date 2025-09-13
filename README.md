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


```


