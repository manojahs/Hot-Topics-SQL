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
```


