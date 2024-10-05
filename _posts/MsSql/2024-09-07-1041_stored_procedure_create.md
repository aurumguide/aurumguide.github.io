---
title: "How to create and use a stored procedure"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql procedure]

## permalink: /MsSql//////////

toc: true
toc_sticky: true
 
date: 2024-09-07
last_modified_at: 2024-09-07
---
 
A stored procedure is a SQL statement that is compiled into the database and stored on the database server.

## <mark>Stored Procedure Description</mark>

- Stored Procedure is a feature supported by most databases and can be thought of as a collection of queries.
- Stored Procedure is stored on the database server and can be reused. In other words, it is useful when you need to use repetitive SQL.
- You can use most of the DML statements such as SELECT, INSERT, UPDATE, DELETE, as well as IF statements, Declare, set, cursors, while statements, dynamicSql statements, etc.
- Since Procedure is basically compiled when first executed, the execution plan can be reused, which greatly helps improve performance.
- Procedure can use input and output parameters, so the user can define the format for the execution result.

![Here is an example of using procedure.](/assets/images/postsImages/MsSql/1041_stored_procedure_create/1.jpg)

## <mark>Executing Stored Procedures and Examples</mark>

### ***How to create and change Stored Procedures.***

- You can create a stored procedure using the CREATE statement.
- Syntax.

```sql
CREATE PROCEDURE <ProcedureName>
   @<ParameterName1> <data type>,
   @<ParameterName2> <data type>
AS   
   SET NOCOUNT ON;
   SELECT <your SELECT statement>;
```

### ***Create a Stored Procedure.***

- create example code.

```sql
DROP TABLE IF EXISTS InfoForProcedure;
CREATE TABLE InfoForProcedure (
    UserId int,
    UserNm varchar(255) NOT NULL,
    UserAge  int ,
    UserAddress varchar(500) 
);
 
INSERT INTO dbo.InfoForProcedure(UserId,UserNm,UserAge,UserAddress) 
VALUES (272, N'Ken',34,'New York')
      ,(273, N'Brian',34,'LA')
      ,(274, N'Stephen',23,'SEOUL')
      ,(275, N'Michael',10,'London')
      ,(276, N'Linda',70,'Bedlin');
-- Create a stored procedure  1
CREATE PROC USP_ProcedureDefault
AS
SELECT DB_NAME() AS dbname;
-- EXECUTE
EXECUTE USP_ProcedureDefault;
 
-- Create a stored procedure 2
CREATE PROC USP_ProcedureSample (
   @paramUserId    int
) AS
SELECT *
  FROM InfoForProcedure
 WHERE UserId  = @paramUserId
;
-- EXECUTE
EXECUTE USP_ProcedureSample 273;
```

### ***Modify Stored Procedure.***

- Here is the modified example code.

```sql
-- Modify a Stored Procedure
ALTER PROC USP_ProcedureSample (
   @paramUserId    int
) AS
  SELECT *
    FROM InfoForProcedure
   WHERE UserId  >  @paramUserId
;
-- EXECUTE
EXECUTE USP_ProcedureSample 273;
```

### ***Recompile Stored Procedure.***

- Recompile source code.

```sql
--- Recompile a Stored Procedure
EXECUTE USP_ProcedureSample 273  WITH RECOMPILE;
```

### ***Rename Stored Procedure.***

- Here is the rename example code.

```sql
--Rename the stored procedure.  
EXEC sp_rename 'sampleDB.dbo.USP_ProcedureSample'
              ,'USP_ProcedureSample_reName';
```

### ***Delete Stored Procedure.***

- This is the drop example code.

```sql
-- Delete a stored procedure
DROP PROCEDURE [USP_ProcedureSample_reName];
```

## <mark>Search for Stored Procedure and Check Content</mark>

### ***Search for Stored Procedure.***

- This is the code to search for the existence of a procedure.

```sql
-- Search procedure name
SELECT name AS procedure_name
   , SCHEMA_NAME(schema_id) AS schema_name
   , type_desc
   , create_date
   , modify_date
FROM sys.procedures
WHERE name = 'USP_ProcedureSample_reName';
```

### How to search procedure contents with a command.

- This is a command to search stored procedure contents.

```sql
-- Check the Storage Procedure Source 1.
sp_helptext 'USP_ProcedureSample';
```

### How to search procedure contents with query.

- Search stored procedure contents

```sql
-- Check the Storage Procedure Source 2.
SELECT OBJECT_NAME(OBJECT_ID) AS  PROC_NM
      ,definition             AS  PROC_SORCE
FROM sys.sql_modules  
WHERE object_id = (OBJECT_ID(N'dbo.USP_ProcedureSample'));
```
