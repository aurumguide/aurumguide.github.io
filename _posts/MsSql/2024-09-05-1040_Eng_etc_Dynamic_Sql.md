---
title: "How to use Dynamic SQL and example query"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql///  aa

toc: true
toc_sticky: true
 
date: 2024-09-05
last_modified_at: 2024-09-05
---
 

Dynamic SQL is a very useful SQL statement that creates and executes SQL statements at runtime based on the input parameters passed.

## <mark>Dynamic SQL Features</mark>

- Dynamic SQL is a programming method that creates SQL queries as strings according to conditions and dynamically executes them at runtime.
- You can dynamically create query statements using the values ​​of variables requested by the program.
- Dynamic SQL can simplify queries and reduce the amount of development source.
- Dynamic SQL strings are executed using EXEC, EXECUTE, and sp\_executesql.

## <mark>How to use Dynamic SQL</mark>

### ***How to use Dynamic SQL for DML statements.***

- Create a DML statement in a string variable and then execute Dynamic SQL.

```sql
DROP TABLE IF EXISTS InfoForDynamicSql;
CREATE TABLE InfoForDynamicSql (
    UserId int,
    UserNm varchar(255) NOT NULL,
    UserAge  int    NULL,
    UserAddress varchar(500)   NULL
);
INSERT INTO dbo.InfoForDynamicSql(UserId,UserNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian')
      ,(274, N'Stephen')
      ,(275, N'Michael')
      ,(276, N'Linda');

--Dynamic SQL for DML statements
DECLARE @dynamicSQL nvarchar(max),
        @_userId nvarchar(200) ,
        @_userAddress  nvarchar(200) 

-- assign values
set @_userId = CAST(272 as nvarchar(200));
set @_userAddress = CAST('New York in USA ' as nvarchar(200));
  
-- build dynamic upate statement
SET @dynamicSQL = ' update InfoForDynamicSql SET UserAddress = ''' 
                  + @_userAddress 
                  + ''' WHERE UserId = ' 
                  +  @_userId ; 

print(@dynamicSQL);
--execute dynamic statement
execute(@dynamicSQL);
```

### ***How to use Dynamic SQL in stored procedures.***

- Create a DML statement in a string variable in PROCEDURE and then execute it. However, try receiving the parameter variable from outside and processing it.

```sql
--Dynamic SQL in stored procedures
CREATE PROCEDURE ProcedureFordynamicSQL(
    @userId int,
    @userAddress nvarchar(200) 
)
AS
BEGIN
  DECLARE @dynamicSQL nvarchar(max),
          @_userId nvarchar(200) ,
          @_userAddress  nvarchar(200)
  -- assign values
  set @_userId = CAST(@userId as nvarchar(200));
  set @_userAddress = CAST(@userAddress as nvarchar(200)); 
    
  SET @dynamicSQL = ' update InfoForDynamicSql SET UserAddress = ''' 
                    + @_userAddress +  ''' WHERE UserId = ' +  @_userId ; 
  print(@dynamicSQL);
  --execute dynamic statement
  execute(@dynamicSQL)
END;

EXECUTE ProcedureFordynamicSQL 273,'seoul the korea';
```

### ***How to use Dynamic SQL for DDL statements.***

- Execute DDL statements that delete and create tables using Dynamic SQL.

```sql
--Dynamic SQL SQL for DDL statements
DECLARE @dynamicSQL nvarchar(max),
        @tablename nvarchar(50),
        @columnsInfo varchar(200);

SET @tablename = 'dbo.InfoForDynamicSql'
SET @columnsInfo  = '(
    UserId int,
    UserNm varchar(255) NOT NULL,
    UserAge  int    NULL,
    UserAddress varchar(500) NULL
) ';

SET @dynamicSQL = N' DROP TABLE IF EXISTS ' + @tablename + ' ; '
SET @dynamicSQL += N' CREATE TABLE ' + @tablename + @columnsInfo + ' ; '

EXECUTE(@dynamicSQL);
```

### ***How to use sp_executesql in PROCEDURE to use Dynamic SQL.***

- Create Dynamic SQL in PROCEDURE and execute it using sp_executesql.

```sql
--Execute Dynamic SQL using sp_executesql
ALTER PROCEDURE PRO_dynamicSqlForSp_executesql 
  @ParamUserId    INT,
  @ParamUserNm    VARCHAR(100),
  @ParamUserAge   INT, 
  @ParamAddress   varchar(500) 
AS  
DECLARE @dynamicSQL NVARCHAR(500);
DECLARE @ParmDefinition NVARCHAR(500);  
  
SET @dynamicSQL = ' INSERT INTO  dbo.InfoForDynamicSql ' 
                    + ' ( UserId,UserNm,UserAge, UserAddress  ) ';
 
SET @dynamicSQL += ' VALUES ( @StmtParamUserId, @StmtParamUserNm, @StmtParamUserAge,@StmtParamAddress);'                  

SET @ParmDefinition =  N'@StmtParamUserId    INT,
                         @StmtParamUserNm    VARCHAR(100),
                         @StmtParamUserAge   INT,
                         @StmtParamAddress    VARCHAR(500)
';
 
EXECUTE sp_executesql @dynamicSQL,  @ParmDefinition,
@ParamUserId,
@ParamUserNm,
@ParamUserAge,
@ParamAddress
  
--  EXECUTE  
EXECUTE PRO_dynamicSqlForSp_executesql
@ParamUserId = '200', 
@ParamUserNm = 'mikle', 
@ParamUserAge = 10,
@ParamAddress = 'La'
;
```

### ***Execute Dynamic SQL using the OUTPUT parameter of sp_executesql.***

- Execute the OUTPUT of Dynamic SQL using sp_executesql.

```sql
-- Use the OUTPUT parameter.
DECLARE @dynamicSQL NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
DECLARE @OutPutUserName NVARCHAR(25);  
DECLARE @IntVariable INT;  

SET @dynamicSQL = N'SELECT @UserNmOUT = MAX(UserNm)  
  FROM InfoForDynamicSql  
  WHERE UserId = @ParamUserId ';  

SET @ParmDefinition = N'@ParamUserId             INT,  
                        @UserNmOUT NVARCHAR(25)  OUTPUT 
                       ';  
 
EXECUTE sp_executesql  
     @dynamicSQL  
    ,@ParmDefinition  
    ,@ParamUserId = 200  
    ,@UserNmOUT =  @OutPutUserName OUTPUT;  

-- SELECT Data.  
SELECT @OutPutUserName  AS OutPutUserName;
```

![ Dynamic SQL](/assets/images/postsImages/MsSql/1040_Eng_etc_Dynamic_Sql/1.png)

## <mark>What are the advantages and disadvantages of Dynamic SQL?</mark>

### ***Advantages of using Dynamic SQL.***

- Dynamic SQL can be used to write queries usefully.
- Dynamic SQL can be used to write queries with patterns simply.

### ***Disadvantages of using Dynamic SQL.***

- Debugging is difficult when errors occur.
- It is difficult to understand the source code compared to static SQL.
- It can cause security problems when strings are concatenated and used.
- Since MSSQL Server must create an execution plan every time Dynamic SQL is executed, performance may be reduced compared to static SQL.
