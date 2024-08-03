---
title: "MSSQL EXECUTE usage method and execution example"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-09-01
last_modified_at: 2024-09-01
---

The MSSQL EXECUTE command is useful for executing stored procedures or passed SQL strings in MSSQL.

## <mark>Features of MSSQL EXECUTE</mark>

### ***Execution of PROCEDURE, FUNCTION.***

- The EXECUTE command is used to execute a system PROCEDURE, a user-defined PROCEDURE, a scalar-valued user-defined FUNCTION, or a Transact-SQL batch.

### ***String, dynamic SQL execution.***

- Useful when executing dynamically generated queries or batches within Transact-SQL statements or user-defined PROCEDUREs written as string combinations.

### ***MSSQL EXECUTE permission.***

- No permission is required to execute an EXECUTE statement, but permission is required for the target referenced within the EXECUTE string.

- That is, if there is an UPDATE or INSERT statement within the EXECUTE string, you must have UPDATE or INSERT permission on the corresponding table.

### ***The result set returned by the execution.***

- You can define the result set returned by using the WITH RESULT SETS option.

### ***Where MSSQL EXECUTE can be used.***

![ MSSQL EXECUTE command](/assets/images/postsImages/MsSql/1038_Eng_etc_EXECUTE/1.png)

## <mark>How to use MSSQL EXECUTE</mark>

### ***Basic syntax of the EXECUTE command in MSSQL.***

- An example of executing a basic statement.

```sql
CREATE TABLE InfoForExecute (
    UserId int,
    UserNm varchar(255) NOT NULL,
    WirteDate  datetime
);
 
INSERT INTO dbo.InfoForExecute(UserId,UserNm,WirteDate) 
VALUES (272, N'Ken',getdate())
 ,(273, N'Brian',getdate())
 ,(274, N'Stephen',getdate())
 ,(275, N'Michael',getdate())
 ,(276, N'Linda',getdate());

-- Basic Syntax of the EXEC Command in SQL Server
-- DROP PROCEDURE IF EXISTS USP_EXECUTE_BASIC;
CREATE PROCEDURE USP_EXECUTE_BASIC
    @UserNm NVARCHAR(10)
AS
BEGIN
    SELECT * FROM InfoForExecute WHERE UserNm = @UserNm;
END;
-- EXECUTE
EXECUTE sp_who;
EXECUTE('SELECT * FROM InfoForExecute;');
EXECUTE USP_EXECUTE_BASIC 'Stephen'; 
EXECUTE USP_EXECUTE_BASIC @UserNm = 'Stephen';
```

### ***Using parameters.***

- You can pass parameters to a stored procedure and execute it.
- You can declare parameters for the procedure, but you can also pass values ​​in order.

```sql
-- Using multiple parameters
-- DROP PROCEDURE IF EXISTS USP_EXECUTE_MULTI_PARAM;
CREATE PROCEDURE USP_EXECUTE_MULTI_PARAM   
   @UserNm NVARCHAR(10),
   @InputDate datetime
AS
BEGIN
  SELECT * 
    FROM InfoForExecute 
  WHERE UserNm = @UserNm
    and WirteDate <  @InputDate ;
END;
-- EXECUTE
DECLARE @InputDate datetime;
SET @InputDate = GETDATE();
EXECUTE dbo.USP_EXECUTE_MULTI_PARAM 'Stephen', @InputDate;
```

### ***Using EXECUTE 'tsql_string' with variables.***

- This is a method of executing a string by dynamically allocating it to a declared variable.

```sql
-- Using EXECUTE 'tsql_string' with variables
declare @tablesStr varchar(100)
       ,@whereStr varchar(100);      
      
 set @tablesStr = '  InfoForExecute ';
 set @whereStr = ' WHERE UserId = 273 ';
 EXECUTE ('SELECT * FROM ' + @tablesStr +  @whereStr + ' ;');
```

### ***Using EXECUTE with stored procedure variables.***

- An example of assigning a stored procedure to a variable and then executing the variable.

```sql
-- Using EXECUTE with Stored Procedure Variables
CREATE PROCEDURE USP_EXECUTE_WITH_PROCEDURE   
AS
BEGIN
    SELECT * FROM InfoForExecute ;
END;
-- EXECUTE
DECLARE @PROC_NM varchar(100);
SET @PROC_NM = 'USP_EXECUTE_WITH_PROCEDURE';
-- EXEC @@PROC_NM;
EXECUTE @PROC_NM;
```

### ***Using EXECUTE with DEFAULT.***

- You can create a DEFAULT value when creating a stored procedure.
- If there is no parameter value when executing the procedure, enter the DEFAULT value.

```sql
-- Using EXECUTE with DEFAULT
-- DROP PROCEDURE dbo.USP_EXECUTE_WITH_Defaults;
CREATE PROCEDURE dbo.USP_EXECUTE_WITH_DEFAULTS (
    @PARAM1 SMALLINT = 42, 
    @PARAM2 CHAR(1), 
    @PARAM3 VARCHAR(20) = 'DEFAULTS EXECUTE')
AS 
   SET NOCOUNT ON;
   SELECT @PARAM1, @PARAM2, @PARAM3
;
-- EXECUTE in various types
-- Specifying a value only for one parameter (@p2).
EXECUTE DBO.USP_EXECUTE_WITH_DEFAULTS @PARAM2 = 'A';
-- Specifying a value for the first two parameters.
EXECUTE DBO.USP_EXECUTE_WITH_DEFAULTS 68, 'B';
-- Specifying a value for all three parameters.
EXECUTE DBO.USP_EXECUTE_WITH_DEFAULTS 68, 'C', 'HOUSE';
-- Using the DEFAULT keyword for the first parameter.
EXECUTE DBO.USP_EXECUTE_WITH_DEFAULTS @PARAM1 = DEFAULT, @PARAM2 = 'D';
-- Specifying the parameters in an order different from the order defined in the procedure.
EXECUTE DBO.USP_EXECUTE_WITH_DEFAULTS DEFAULT, @PARAM3 = 'LOCAL', @PARAM2 = 'E';
-- Using the DEFAULT keyword for the first and third parameters.
EXECUTE DBO.USP_EXECUTE_WITH_DEFAULTS DEFAULT, 'H', DEFAULT;
EXECUTE DBO.USP_EXECUTE_WITH_DEFAULTS DEFAULT, 'I', @PARAM3 = DEFAULT;
```

### ***Using EXECUTE with user-defined functions.***

- You can assign the value returned from the function to a variable and then RETURN it.

```sql
-- Using EXECUTE with user-defined functions
-- drop FUNCTION FN_USER_DEFINE_EXECUTE;
CREATE FUNCTION FN_USER_DEFINE_EXECUTE(@InYear INT)
RETURNS INT
AS
BEGIN
    DECLARE @Year INT
    SET @Year = YEAR(GETDATE()) - @InYear
    RETURN (@Year)
END
-- EXECUTE 
DECLARE @ReturnVal nvarchar(50);
SET @ReturnVal = NULL;
EXEC @ReturnVal = dbo.FN_USER_DEFINE_EXECUTE @InYear = '2020';
SELECT @ReturnVal;
```

### ***Redefining the DATA returned using EXECUTE.***

- You can redefine a single result set using MSSQL EXECUTE.
- You can define SELECT result DATA in a procedure.
- You can redefine two result sets using MSSQL EXECUTE.
- You can define more than one SELECT result DATA in a single procedure.

```sql
--Redefine a single result set using EXECUTE
CREATE PROCEDURE USP_EXECUTE_RESULT_SETS
    @UserNm NVARCHAR(10)
AS
BEGIN
   SELECT UserId,UserNm FROM InfoForExecute ;
END;
--EXECUTE
EXEC USP_EXECUTE_RESULT_SETS 'Brian' 
WITH RESULT SETS  
(   
   (
    [Reporting Level] INT  NULL,  
    [ID of Employee] VARCHAR(100)  NULL
   )    
);

-- Redefine two result sets using EXECUTE
-- DROP PROCEDURE USP_EXECUTE_MULTI_RESULT_SETS
CREATE PROCEDURE USP_EXECUTE_MULTI_RESULT_SETS
    @UserNm NVARCHAR(10)
AS
BEGIN
     SELECT UserId FROM InfoForExecute ;
     SELECT UserId,UserNm FROM InfoForExecute ;
END;
-- EXECUTE
EXECUTE USP_EXECUTE_MULTI_RESULT_SETS 'Ken' 
WITH RESULT SETS  
(  
   (
    [onReurnData] INT  NULL 

   ),
   (
    [Reporting Level] INT  NULL,  
    [ID of Employee] VARCHAR(100)  NULL
   )    
);
```

## <mark>Precautions when using MSSQL EXECUTE.</mark>

- Be careful when executing Transact-SQL statements with string combinations, as you may be exposed to malicious attacks.
- It is recommended to execute MSSQL EXECUTE by receiving values ​​as parameters or using sp_executesql.
