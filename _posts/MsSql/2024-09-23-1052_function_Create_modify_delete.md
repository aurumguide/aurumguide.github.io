---
title: "MSSQL Function Creation, Modification, Deletion Description and Usage"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Function]

## permalink: /MsSql//////////

toc: true
toc_sticky: true
 
date: 2024-09-23
last_modified_at: 2024-09-23
---
 
There are functions supported by the MSSQL system, such as system functions, but users can create, modify, and delete functions when necessary.

## <mark>MSSQL function creation, modification, and deletion explanation</mark>

- Functions that users create directly are usually called user-defined functions.
- System functions cannot be changed.
- User-defined functions can be created, modified, and deleted in the form of scalar functions and table-valued functions.
- Using user-defined functions, you can make repeated QUERY statements into a single function and call them when necessary, making QUERY statements concise and readable.
- Table-valued functions are functions that return DATA in table format.
- Scalar functions can return a single value.

## <mark>MSSQL function usage</mark>

### ***User-defined function flow chart.***

![Description and usage of creating, modifying, and deleting functions.](/assets/images/postsImages/MsSql/1052_function_Create_modify_delete/1.png)

### **Creating a User-Defined Function.**

- Creating a User-Defined Function

```sql
-- Creating a Scalar Function.
CREATE FUNCTION dbo.AurumGuide_scalarFunc (@PARAM1 varchar(10), @PARAM2 varchar(10))
RETURNS varchar(100)
AS
BEGIN
    declare @returnStr varchar(100);
    set @returnStr  = 'Hello World ' + @PARAM1 + @PARAM2;
    RETURN @returnStr
END;
go
-- EXCUTE 
select dbo.AurumGuide_scalarFunc('Aurum','Guide');
```

- Create a table-valued function.

```sql
-- CREATE TABLE
USE sampleDB;
DROP TABLE IF EXISTS AurumGuideForFunc;
CREATE TABLE AurumGuideForFunc (
    UserId int,
    UserNm varchar(255) NOT NULL    
);
INSERT INTO dbo.AurumGuideForFunc(UserId,UserNm ) 
VALUES (272, N'Ken' )	 
      ,(274, N'Stephen' )
      ,(275, N'Michael' )
      ,(276, N'Linda' )
      ,(277, N'Lee' ) 
   ;
DROP TABLE IF EXISTS AurumGuideForFuncJoin;
CREATE TABLE AurumGuideForFuncJoin (
    UserId int,    
    UserAge  int     NULL,
    DeptId varchar(500)   NULL
); 
INSERT INTO dbo.AurumGuideForFuncJoin(UserId,UserAge,DeptId) 
VALUES (272 ,10,100)
      ,(274 ,20,100)
      ,(275 ,30,200)
      ,(276 ,40,200)
      ,(277 ,40,100) 
; 
 --   Table returning function
CREATE FUNCTION fn_AurumGuide_ReturnUserFunc (@DeptId INT)
RETURNS @Return_AurumGuide  TABLE
(
  UserId int,
  UserNm varchar(255)    NULL,
  UserAge  int           NULL,
  DeptId varchar(500)    NULL
)
AS
BEGIN
  INSERT @Return_AurumGuide
  SELECT af1.UserId,af1.UserNm,af2.UserAge,af2.DeptId
  FROM AurumGuideForFunc af1
  join AurumGuideForFuncJoin af2
  on af1.UserId = af2.UserId
  WHERE af2.DeptId = @DeptId;
  RETURN;
END;

go
-- EXCUTE
select * 
from fn_AurumGuide_ReturnUserFunc(100);
```

#### **Modify User Defined Functions.**

- Example of Modifying Functions.

```sql
-- Modify MSSQL Functions
GO
ALTER FUNCTION dbo.AurumGuide_scalarFunc (
@PARAM1 varchar(10),
@PARAM2 varchar(10),
@PARAM3 varchar(10)
)
RETURNS varchar(100)
AS
BEGIN
  declare @returnStr varchar(100);
    set @returnStr  = 'Hello World ' + @PARAM1 + @PARAM2 + ' ' + @PARAM3;
  RETURN @returnStr
END;
go
-- EXCUTE 
select dbo.AurumGuide_scalarFunc('Aurum','Guide','Good');
```

### ***Delete User Defined Function.***

- Example of deleting a function.

```sql
-- Deleting a MSSQL function
DROP FUNCTION dbo.AurumGuide_scalarFunc;
-- CHECK
select dbo.AurumGuide_scalarFunc('Aurum','Guide','Good');
```

## <mark>When to use MSSQL functions?</mark>

- If you have a repetitive SQL statement, you may want to consider whether you can make it into a Function.
- When creating a function, it would be better if you could make it into a scalar function.
- If you select and return data inside the function, there may be a performance issue.
