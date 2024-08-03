---
title: "How to use Union, Union ALL and the differences"
excerpt: ""

categories:
  - MsSql
tags:
  - [clause]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-08-06
last_modified_at: 2024-08-06
---
 

Union(all) allows you to retrieve data by combining columns from multiple select statements into a single select statement.

## <mark>UNION, UNION ALL 차이점</mark>

### ***UNION features.***

- Removes duplicate values ​​and outputs the results as a union.
- The column names must be the same, and the data types for each column must be the same to avoid errors.
- Removes duplicate values ​​and outputs data.
- In terms of performance, it is slower than UNION ALL because it performs duplicate removal internally.

### ***UNION ALL features.***

- It outputs the result as a union set while maintaining duplicate values.
- It merges the query results and returns them as a single result set.
- As with UNION, the column names must be the same, and the data types for each column must be the same to avoid errors.
- In terms of speed, Union All is faster than Union because it requires one more operation to remove duplicate values.

## <mark>How to use UNION, UNION ALL</mark>

### ***UNION, UNION ALL  syntax.***

- The number of columns in colName must be the same, and the data types must also be the same for each column.

```sql
select colName1,colName2,colName3
  from tableName1
 union (all)
select colName1,colName2,colName3
  from tableName2;
```

### ***Use UNION, UNION ALL.***

- Compare the data of UNION and UNION ALL to see the differences.

```sql
-- UNION  
SELECT EmpID,EmpName,Title,DeptID
FROM UserInfoForUnionOne
UNION 
SELECT EmpID,EmpName,Title,DeptID
FROM UserInfoForUnionTwo;

-- UNION ALL  
SELECT EmpID,EmpName,Title,DeptID
FROM UserInfoForUnionOne
UNION ALL
SELECT EmpID,EmpName,Title,DeptID
FROM UserInfoForUnionTwo;
```

![Compare the data of UNION and UNION ALL to see the differences.](/assets/images/postsImages/MsSql/1025_Eng_clause_UNION_UNIONALL/1.png)

### ***Using UNION with ORDER BY.***

- If you want to use ORDER BY in UNION, you must use it at the very end. That is, you cannot use ORDER BY individually.

```sql
-- order by --> error
SELECT EmpID,EmpName,Title,DeptID
FROM UserInfoForUnionOne
order by  EmpID
UNION ALL
SELECT EmpID,EmpName,Title,DeptID
FROM UserInfoForUnionTwo
order by  EmpID;

-- order by --> good
SELECT EmpID,EmpName,Title,DeptID
FROM UserInfoForUnionOne
UNION ALL
SELECT EmpID,EmpName,Title,DeptID
FROM UserInfoForUnionTwo
order by  EmpID DESC;
```

![Using UNION with ORDER BY.](/assets/images/postsImages/MsSql/1025_Eng_clause_UNION_UNIONALL/2.png)

## ***data for UNION***

- sql for example

```sql
DROP TABLE IF EXISTS UserInfoForUnionOne;
CREATE TABLE dbo.UserInfoForUnionOne
(
   EmpID SMALLINT NOT NULL, 
   EmpName NVARCHAR(40) NOT NULL,
   Title NVARCHAR(50) NOT NULL,
   DeptID SMALLINT NOT NULL 
);

DROP TABLE IF EXISTS UserInfoForUnionTwo;
CREATE TABLE dbo.UserInfoForUnionTwo
(
  EmpID SMALLINT NOT NULL, 
  EmpName NVARCHAR(40) NOT NULL,
  Title NVARCHAR(50) NOT NULL,
  DeptID SMALLINT NOT NULL 
);
  
 INSERT INTO UserInfoForUnionOne VALUES 
(110,'Jessica','Pacific Branch Manager','10'),
(111,'Alicia','Pacific Branch','10'),
(120,'Michael','Pacific Branch Manager','20'),
(121,'Alicia','US Branch','20') ;

 INSERT INTO UserInfoForUnionTwo VALUES  
(120,'Michael','Pacific Branch Manager','20'),
(121,'Alicia','US Branch','20'),
(130,'Laura','Marketing Team Leader','30'),
(131,'Stephen','Marketing Team','30');
```
