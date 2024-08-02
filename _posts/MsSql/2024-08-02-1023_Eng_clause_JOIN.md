---
title: "MSSQL JOIN type description and usage"
excerpt: ""

categories:
  - MsSql
tags:
  - [clause]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-08-02
last_modified_at: 2024-08-02
---

MSSQL Join is used to connect two or more tables using columns to output data stored in the tables.  
Usually, two tables are joined using a primary key or foreign key to improve performance.

## <mark>MSSQL Join Features</mark>

Rather than writing down the features of INNER JOIN, LEFT OUTER JOIN, etc., I will first explain them with a picture using the concept of a set.

![Display](/assets/images/postsImages/MsSql/1023_Eng_clause_JOIN/1.png)

## <mark>MSSQL Join type description and usage</mark>

### ***How to use MSSQL Join INNER JOIN.***

- Since it only shows the same thing, you can think of it as an intersection.
- In other words, it is used to output only duplicate values.

```sql
-- INNER JOIN
SELECT A.EmpID,A.EmpName,A.DeptID,B.DeptID,B.DeptName
FROM UserInfoForJoin A
INNER JOIN DeptInfoForJoin B 
ON A.DeptID = B.DeptID
```

![How to use MSSQL Join INNER JOIN.](/assets/images/postsImages/MsSql/1023_Eng_clause_JOIN/2.png)

### ***How to use MSSQL Join LEFT OUTER JOIN.***

- Since MSSQL Join is performed based on the left table, the output value is used when searching for all data in the ta table and overlapping data in the ta table and tb table.

```sql
-- LEFT OUTER JOIN
SELECT A.EmpID,A.EmpName,A.DeptID,B.DeptID,B.DeptName
FROM UserInfoForJoin A
LEFT OUTER JOIN DeptInfoForJoin B 
ON A.DeptID = B.DeptID
```

![LEFT OUTER JOIN](/assets/images/postsImages/MsSql/1023_Eng_clause_JOIN/3.png)

### ***How to use MSSQL Join RIGHT OUTER JOIN.***

- You can think of it as the opposite of LEFT OUTER JOIN.
- Used to search all data in the tb table and overlapping data in the ta table and tb table.

```sql
-- RIGHT OUTER JOIN
SELECT A.EmpID,A.EmpName,A.DeptID,B.DeptID,B.DeptName
 FROM UserInfoForJoin A
RIGHT OUTER JOIN DeptInfoForJoin B 
ON A.DeptID = B.DeptID
```

![RIGHT OUTER JOIN](/assets/images/postsImages/MsSql/1023_Eng_clause_JOIN/4.png)

### ***How to use MSSQL Join FULL OUTER JOIN.***

- All data contained in the ta table and tb table are searched.
- Mathematically, you can think of it as the union of sets.

```sql
--FULL OUTER JOIN
SELECT A.EmpID,A.EmpName,A.DeptID,B.DeptID,B.DeptName
 FROM UserInfoForJoin A
FULL OUTER JOIN DeptInfoForJoin B 
ON A.DeptID = B.DeptID
```

![FULL OUTER JOIN](/assets/images/postsImages/MsSql/1023_Eng_clause_JOIN/5.png)

### ***How to use MSSQL Join CROSS JOIN.***

- Used to output all the numbers of cases.
- If the standard table is ta, you can think of joining one row of data from ta with all rows of the tb table.
- The number of rows resulting from the join is output as the number multiplied by the number of rows in each table.

```sql
--CROSS JOIN
SELECT  A.EmpID,A.EmpName,A.DeptID,B.DeptID,B.DeptName
FROM UserInfoForJoin A
CROSS JOIN DeptInfoForJoin B;
```

![CROSS JOIN](/assets/images/postsImages/MsSql/1023_Eng_clause_JOIN/6.png)

### ***How to use MSSQL Join SELF JOIN.***

- In the sense of joining with yourself, you can think of it as repeatedly joining one table.

```sql
--SELF JOIN
SELECT A.DeptID,A.DeptName ,B.DeptID,B.DeptName
FROM DeptInfoForJoin a 
    ,DeptInfoForJoin b
```

![SELF JOIN](/assets/images/postsImages/MsSql/1023_Eng_clause_JOIN/7.png)

## <mark>Preparation source for Join practice</mark>

```sql
USE sampleDB;
-- Employee information
DROP TABLE IF EXISTS UserInfoForJoin;
CREATE TABLE dbo.UserInfoForJoin
(
    EmpID SMALLINT NOT NULL, 
    EmpName NVARCHAR(40) NOT NULL,
    Title NVARCHAR(50) NOT NULL,
    DeptID SMALLINT NOT NULL 
);

-- Department information
DROP TABLE IF EXISTS DeptInfoForJoin;
CREATE TABLE dbo.DeptInfoForJoin
(
    DeptID SMALLINT NOT NULL, 
    DeptName NVARCHAR(40) NOT NULL 
);


INSERT INTO UserInfoForJoin VALUES
    (100,'Elizabeth','Vice President',''),
    (101,'Ashley','Vice President',''),
    (110,'Jessica','Pacific Branch Manager','10'),
    (111,'Alicia','Pacific branch','10'),
    (112,'Diana','Pacific branch','10'),
    (120,'Michael','US branch manager','20'),
    (121,'Alicia','US branch','20'),
    (122,'Diana','US branch','20'),
    (130,'Laura','Marketing Team Leader','30'),
    (131,'Stephen','Marketing Team','30'),
    (132,'David','Marketing Team','30')
;

INSERT INTO DeptInfoForJoin VALUES
    (10,'Pacific branch'),
    (20,'US branch'),
    (30,'Marketing Team'),
    (40,'Audit team')
;
```
