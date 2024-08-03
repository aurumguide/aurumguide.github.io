---
title: "How to use and features of MSSQL PIVOT, UNPIVOT"
excerpt: ""

categories:
  - MsSql
tags:
  - [MSSQL FUNCTION]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-08-10
last_modified_at: 2024-08-10
---

PIVOT converts the row set of retrieved data into columns and displays the result data, while UNPIVOT converts the column data into rows and outputs them.

## <mark>PIVOT, UNPIVOT Description and Advantages, Disadvantages</mark>

### ***PIVOT Description.***

- The PIVOT function is used to convert rows of data into columns.
- PIVOT uses aggregate functions to output the results of the data.
- Aggregation functions can use sum(), count(), ave(), etc.
- The target of PIVOT can be specified using FOR.
- The rows that come after the IN() clause are the targets to be converted into columns.

### ***Description of UNPIVOT***

- UNPIVOT is easy to understand if you think of it as the opposite of PIVOT.
- The UNPIVOT function is used to convert a column of data into a row set.
- FOR the column specified in the IN() clause and use UNPIVOT to specify it as one row.
- It is not often used in the field, but I hope you understand the concept and move on.

### ***Advantages of PIVOT and UNPIVOT.***

- The PIVOT function requires defined columns, so it is only possible statically. Of course, you can use dynamic SQL, but there are restrictions on columns.
- The PIVOT and UNPIVOT functions cause performance problems when processing large amounts of data.
- Queries written with PIVOT and UNPIVOT are more complex than you might think, so it may take a lot of time to analyze and write the source.

### ***Disadvantages of PIVOT and UNPIVOT.***

- The PIVOT function requires defined columns, so it is only possible statically. Of course, you can use dynamic SQL, but there are restrictions on columns.
- The PIVOT and UNPIVOT functions cause performance problems when processing large amounts of data.
- Queries written with PIVOT and UNPIVOT are more complex than you might think, so it may take a lot of time to analyze and write the source.

## <mark>How to use PIVOT, UNPIVOT</mark>

### ***How to use PIVOT.***

- Use the data stored in the DeptInfoForPivot table.
- Use the PIVOT function to change the salary specified in rows into columns and calculate the sum of the salary by branch.
- This is the source code.

```sql
DROP TABLE IF EXISTS DeptInfoForPivot;
CREATE TABLE DeptInfoForPivot
(
  EmpId  SMALLINT NOT NULL, 
  DeptID SMALLINT NOT NULL, 
  DeptName NVARCHAR(40) NOT NULL,
  MonthPay INT NOT NULL
); 
  
 INSERT INTO DeptInfoForPivot VALUES 
(100,10 ,'Pacific Branch Team','100'),
(101,10 ,'Pacific Branch Team','110'),
(102,10 ,'Pacific Branch Team','120'),
(200,20 ,'Atlantic Point','400'),
(300,30 ,'Indian Ocean Point','500'),
(400,40 ,'US Branch','700') ;
--  PIVOT
SELECT *
FROM (
  SELECT DeptName, DeptID, MonthPay
  FROM DeptInfoForPivot
) AS result
PIVOT (
   SUM(MonthPay) FOR DeptID IN ([10], [20], [30], [40], [50])
) AS pivot_result
ORDER BY DeptName;
```

- Here are the results of the practice.

![How to use PIVOT.](/assets/images/postsImages/MsSql/1027_Eng_Fnc_PIVOT_UNPIVOT/1.png)

### ***How to use UNPIVOT.***

- Use the data stored in the DeptInfoForUnPivot table.
- Use the UNPIVOT function to output the columns Dept_10, Dept_20, Dept_30, and Dept_40 as rows.
- This is the source code.

```sql
DROP TABLE IF EXISTS DeptInfoForUnPivot;
CREATE TABLE DeptInfoForUnPivot
(
    EmpId  SMALLINT NOT NULL, 
    DeptID SMALLINT NOT NULL, 
    DeptName NVARCHAR(40) NOT NULL,
    MonthPay INT NOT NULL,
    Dept_10 NVARCHAR(40) NULL,
    Dept_20 NVARCHAR(40) NULL,
    Dept_30 NVARCHAR(40) NULL,
    Dept_40 NVARCHAR(40) NULL,
); 
 INSERT INTO DeptInfoForUnPivot VALUES 
(100,10 ,'Pacific Branch Team','100','Dept_1X','','',''),
(101,10 ,'Pacific Branch Team','101','Dept_1X','','',''),
(102,10 ,'Pacific Branch Team','102','Dept_1X','','',''),
(200,20 ,'Atlantic Point','200','','Dept_2X','',''),
(300,30 ,'Indian Ocean Point','300','','','Dept_3X',''),
(400,40 ,'US Branch','400','','','','Dept_4X') ;

-- UNPIVOT
SELECT DeptName,DeptTile,unPivotRow
FROM (
    SELECT DeptName,Dept_10, Dept_20, Dept_30, Dept_40
    FROM DeptInfoForUnPivot
) AS result
UNPIVOT (
   unPivotRow FOR DeptTile IN (Dept_10, Dept_20, Dept_30, Dept_40)
) AS unpivot_result
WHERE unPivotRow <> ''
order by unPivotRow, DeptTile;
```

- Here are the results of the practice.

![How to use UNPIVOT.](/assets/images/postsImages/MsSql/1027_Eng_Fnc_PIVOT_UNPIVOT/2.png)

## <mark>Concluding the explanation of PIVOT and UNPIVOT</mark>

- MSSQL provides transformation functions for data through UNPIVOT and PIVOT functions, but they are not used often.
- For simple PIVOT functions, we recommend using CASE statements.
- If the column values ​​of the PIVOT target change dynamically, you can use dynamic SQL.

