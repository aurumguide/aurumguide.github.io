---
title: "How to use MSSQL join hint and join execution principle"
excerpt: ""

categories:
  - MsSql
tags:
  - [clause]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-08-08
last_modified_at: 2024-08-08
---

A join hint can be specified in the From clause of a query. In SQL Server, you can force a join method between tables to optimize the query, and thus specify the join order between tables.

## <mark>Features of join hints</mark>

- You cannot use nested loop join, merge join, and hash join at the same time, and you must select only one.
- You can specify the order of the tables applied to the join, so you can use it efficiently for performance optimization.
- When applying a join hint, you must know the exact size and index of the table to optimize performance, so it is recommended for beginners to use default.

## <mark>Join hint execution principle</mark>

### ***Description of the Nested Loop join hint.***

- Nested Loop join performs a join by sequentially merging rows on both sides sorted by each key.
- Join is performed by scanning both tables simultaneously.
- Nested Loop performs efficiently in joins between large and small tables.
- Performance issues occur in joins between large and large tables.

```sql
 --  LOOP  JOIN,  Execution Plan Shortcuts:  Ctrl  + M
SELECT*
FROM UserInfoForJoinHint AS emp  
INNER LOOP JOIN DeptInfoForJoinHint AS dept 
ON emp.DeptID = dept.DeptID ;
```

![Description of the Nested Loop join hint.](/assets/images/postsImages/MsSql/1026_Eng_clause_JOIN_HINT/1.png)

### ***Description of the merge join hint.***

- Each table accesses its entire processing range, sorts it, and stores it in storage space.
- Partial range processing is not possible, and full range processing is always performed.
- Merge join is effective when the table to be stored and the table to be queried have the same shape.
- Merge join is effective in joins between large tables.

```sql
--  MERGE  JOIN,  Execution Plan Shortcuts:  Ctrl  + M
SELECT*
FROM UserInfoForJoinHint AS emp  
INNER MERGE  JOIN DeptInfoForJoinHint AS dept 
ON emp.DeptID = dept.DeptID ;
```

![Description of the merge join hint.](/assets/images/postsImages/MsSql/1026_Eng_clause_JOIN_HINT/2.png)

### ***Hash join hint explanation.***

- Hash join cannot process partial ranges, and always uses a full scan.
- It is processed in a hash manner, and is mainly executed in a scan manner.
- Hash join is efficient when the table to be saved and the table to be queried are the same.
- Hash join is effective in joins between large tables, but performance problems may occur in joins between small tables and small tables.

```sql
--  HASH  JOIN,  Execution Plan Shortcuts:  Ctrl  + M
 SELECT*
FROM UserInfoForJoinHint AS emp  
INNER HASH  JOIN DeptInfoForJoinHint AS dept 
ON emp.DeptID = dept.DeptID;
```

![Hash join hint explanation.](/assets/images/postsImages/MsSql/1026_Eng_clause_JOIN_HINT/3.png)

## <mark>Concluding the join hint</mark>

### ***join hint basic Query.***

- Here is an example source.

```sql
USE sampleDB;
-- join hint 
DROP TABLE IF EXISTS DeptInfoForJoinHint;
CREATE TABLE dbo.DeptInfoForJoinHint
(
    DeptID SMALLINT NOT NULL, 
    DeptName NVARCHAR(40) NOT NULL,
    CONSTRAINT PK_DeptID PRIMARY KEY CLUSTERED (DeptID ASC)
);

DROP TABLE IF EXISTS UserInfoForJoinHint;
CREATE TABLE dbo.UserInfoForJoinHint
(
    EmpID SMALLINT NOT NULL, 
    EmpName NVARCHAR(40) NOT NULL,
    Title NVARCHAR(50) NOT NULL,
    DeptID SMALLINT NOT NULL,
    CONSTRAINT PK_EmpID PRIMARY KEY CLUSTERED (EmpID ASC),
    CONSTRAINT FK_DeptID FOREIGN KEY (DeptID) REFERENCES dbo.DeptInfoForJoinHint (DeptID)
);

INSERT INTO DeptInfoForJoinHint VALUES
(10,'Pacific Branch'),
(20,'US Branch'),
(30,'Marketing Team'),
(40,'Security Team')
;
  
 INSERT INTO UserInfoForJoinHint VALUES 
(110,'Jessica','Pacific Branch Manager','10'),
(111,'Alicia','Pacific Branch','10'),
(120,'Michael','US Branch Manager','20'),
(121,'Alicia','US Branch','20') ;
```

### ***Join hint caution.***

- Misusing join hints can cause database problems in the future. 
- You should accurately understand the size and structure of tables and indexes and use them when necessary.
