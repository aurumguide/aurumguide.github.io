---
title: "Usage and features of stored procedure Return"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql 

toc: true
toc_sticky: true
 
date: 2024-09-19
last_modified_at: 2024-09-19
---
 
 In MSSQL stored procedure, you can use the Return statement to return a specific value as an integer.

## <mark>Stored procedure Return features</mark>

- When the Return statement returns from the stored procedure, the program that called the procedure checks the return value and decides whether to stop the program or ignore it.
- In the stored procedure, you can return a specific status value by using the Return statement without using the output parameter.
- The return value of the stored procedure Return can only be an integer (int).
- If the Return succeeds, it generally returns 0.

## <mark>How to use the Stored procedure Return</mark>

### ***How to return a return value in a stored procedure.***

- To receive the stored procedure Return, you can check it after declaring the variable.

![This is the source code description of the stored procedure return statement.](/assets/images/postsImages/MsSql/1050_Stored_Procedure_reurn/1.png)

- Here is the example source code.

```sql
DROP TABLE IF EXISTS UserInfoForProcedureReturn;
CREATE TABLE UserInfoForProcedureReturn (
    UserId int,
    UserNm varchar(255) NOT NULL,
    UserAge  int    NULL,
    UserAddress varchar(500)  NULL
);
-- DATA INSERT
INSERT INTO dbo.UserInfoForProcedureReturn(UserId,UserNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian')
      ,(274, N'Stephen')
      ,(275, N'Michael')
      ,(276, N'Linda');
GO 
------------------------------------------------------------------------------
--return basic
 -- DROP PROCEDURE STORE_PROCEDURE_RETURN_EX1;
CREATE  PROCEDURE [DBO].[STORE_PROCEDURE_RETURN_EX1] (      
     @UserId  int
) AS
BEGIN
    SELECT *
      FROM UserInfoForProcedureReturn R1
     where R1.UserId = @UserId ;

    IF(@@ROWCOUNT > 0 )
       RETURN(0);
    ELSE 
       RETURN(-1);
END;

-- EXCUTE  
DECLARE @return_status int;  
EXEC @return_status =  [STORE_PROCEDURE_RETURN_EX1] @UserId = '173' ; 
SELECT 'Return Status' = @return_status;  

-- EXCUTE   
DECLARE @return_status int;  
EXEC @return_status =  [STORE_PROCEDURE_RETURN_EX1] @UserId = '273' ; 
SELECT 'Return Status' = @return_status;
```

### ***How to return a value using a SELECT statement inside Return.***

- It is possible if the DATA returned by Return is an integer type.
- Basically, let's calculate COUNT and return it.

```sql
-- How to return a result using a SELECT clause in a return statement
CREATE PROCEDURE [DBO].[STORE_PROCEDURE_RETURN_EX3] (      
    @UserId  int
) AS
BEGIN
 
RETURN(SELECT isnull(count(*),0) cnt
         FROM UserInfoForProcedureReturn R1
         WHERE R1.UserId = @UserId
      );  
END;

-- EXCUTE  
DECLARE @return_status int;  
EXEC @return_status =  [STORE_PROCEDURE_RETURN_EX3] @UserId = '1' ; 
SELECT 'Return Status' = @return_status;
```

### ***What happens if you return a string when returning?***

- An error occurs because the return value of the stored procedure can only be an integer type.
- Msg 245, Level 16, State 1, Procedure STORE_PROCEDURE_RETURN_EX2, Line 12 [Batch Start Line 96]  
- Failed to convert varchar value 'OK' to data type int.

```sql
-- If the return value contains a string.
CREATE  PROCEDURE [DBO].[STORE_PROCEDURE_RETURN_EX2] (      
   @UserId  int
) AS
BEGIN
    SELECT *
      FROM UserInfoForProcedureReturn R1
   WHERE R1.UserId = @UserId ;

    IF(@@ROWCOUNT > 0 )    
       RETURN('OK');    
    ELSE 
       RETURN('ERROR');
END;

-- EXCUTE  
DECLARE @return_status VARCHAR(200);  
EXEC @return_status =  [STORE_PROCEDURE_RETURN_EX2] @UserId = '273' ; 
SELECT 'Return Status' = @return_status;
```

## <mark>What are other ways to return data?</mark>

- Functions are mainly used to return data.
- User-defined functions include scalar functions and table-valued functions.
- The RETURN statement is used to return arbitrary status values ​​from stored procedures that are relatively simple compared to user-defined functions.
