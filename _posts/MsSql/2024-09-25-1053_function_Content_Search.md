---
title: "How to call MSSQL functions and search their contents"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql//////////

toc: true
toc_sticky: true
 
date: 2024-09-25
last_modified_at: 2024-09-25
---
 If you have created a function in MSSQL, you can call the function to check the return value and how to search the content you have written.

## <mark>Function Call Method and Content Search Description<mark>

### ***Function Call Method.***

- Scalar functions return a single value, so you can call the function in the SELECT clause.
- Table-valued functions return a result set, so you can use them in the FROM clause.
- As the name suggests, they are returned in table form, so you can join and use WHERE conditions.

### ***Function Content Search Description.***

- MSSQL supports the function content search function so that you can check the content of the function you created.
- You can also check the content using SQL Server Management Studio (SSMS).
- It is similar to searching for a string in PROCEDURE.

## <mark>Function Call Method</mark>

### ***Scalar Function Creation and Call Method.***

- Scalar function usage example.

```sql
CREATE FUNCTION dbo.AurumGuide_scalarFuncCall (
  @PARAM1 varchar(10),
  @PARAM2 varchar(10) 
)
RETURNS varchar(100)
AS
BEGIN
  declare @returnStr varchar(100);
  set @returnStr  =   @PARAM1 + @PARAM2;
  RETURN @returnStr
END;
go
-- EXCUTE 
select dbo.AurumGuide_scalarFuncCall('Aurum','Guide');
```

### ***How ​​to create and call a table-valued function.***

- Example of using a table-valued function.

```sql
-- How to create and call a table-valued function.
-- 1. CREATE TABLE   
DROP TABLE IF EXISTS AurumGuideForTableFuncCall;
CREATE TABLE AurumGuideForTableFuncCall(
    UserId int,
    UserNm varchar(255) NOT NULL 
); 

INSERT INTO dbo.AurumGuideForTableFuncCall(UserId,UserNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian')
      ,(274, N'Stephen')
      ,(275, N'Michael')
      ,(376, N'Linda');

GO
-- Table returning function
-- DROP FUNCTION fn_AurumGuide_TableFuncCall;
CREATE FUNCTION fn_AurumGuide_TableFuncCall(@UserNm varchar(255))
RETURNS @Return_AurumGuide  TABLE
(
  UserId int,
  UserNm varchar(255)    NULL 
)
AS
BEGIN
  INSERT @Return_AurumGuide
  SELECT *
  FROM AurumGuideForTableFuncCall af	 
  WHERE af.UserNm like '%' +  @UserNm  +'%';
  RETURN;
END;

-- EXCUTE
select * 
from fn_AurumGuide_TableFuncCall('a') fnCall
where fnCall.UserId like '%27%'
;
```

## <mark>Function Content Search Method</mark>

### ***Content Search Flow Chart.***

![This is a flow chart for searching function contents.](/assets/images/postsImages/MsSql/1053_function_Content_Search/1.png)

### ***How ​​to use the sp_helptext command.***

- Use the sp_helptext command to search for the contents of a single function.

```sql
EXEC sp_helptext 'fn_AurumGuide_TableFuncCall';
```

### ***Use the TEXT column of syscomments.***

- syscomments is a system view, and you can search for function contents through the text column.

```sql
SELECT type, name, text
  FROM syscomments S1
 JOIN sys.objects S2 
  ON S1.id = S2.object_id
WHERE S2.type IN ('FN','TF')
  AND S1.text LIKE '%AurumGuide%';
```

### ***Search content using INFORMATION_SCHEMA.ROUTINES.***

- You can also use INFORMATION_SCHEMA.ROUTINES schema information.

```sql
SELECT ROUTINE_NAME, ROUTINE_DEFINITION
FROM INFORMATION_SCHEMA.ROUTINES
WHERE ROUTINE_TYPE = 'FUNCTION'
  AND ROUTINE_DEFINITION  LIKE '%fn_AurumGuide%'
ORDER BY ROUTINE_NAME;
```

### ***Searching function contents using sys.objects.***

- sys.objects can search almost all contents of functions, procedures, and triggers.

```sql
SELECT OBJECT_NAME(object_id)  FUNC_NAME
      ,OBJECT_DEFINITION(object_id) FUNC_TEXT
 FROM sys.objects
 WHERE  OBJECT_NAME(object_id) = 'fn_AurumGuide_TableFuncCall'
```

#### **Search Function contents using sys.sql_modules.**

- Mainly used when searching all contents such as Function, Procedure, Trigger, Function, etc.

```sql
SELECT OBJECT_NAME(OBJECT_ID) AS  FUNC_NM
      ,definition             AS  FUNC_SORCE
      ,*
FROM sys.sql_modules  
WHERE object_id = (OBJECT_ID(N'dbo.fn_AurumGuide_TableFuncCall'));
```
