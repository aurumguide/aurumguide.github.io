---
title: "MSSQL Deterministic and Non-Deterministic Functions Explained"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql///

toc: true
toc_sticky: true
 
date: 2024-10-03
last_modified_at: 2024-10-03
---
 
For readers who have a hard time distinguishing between usable and impossible grammars when implementing functions, I will explain the exact concept of deterministic and nondeterministic functions.

## <mark>Comparison of deterministic and nondeterministic functions.</mark>

### ***Deterministic functions.***

- It is easy to understand if you think of deterministic functions as meaning that data is determined from the database's perspective and can return results.
- In other words, a representative deterministic function is the SUM() function.
- A database is a sum of determined tables and columns.

### ***Nondeterministic functions.***

## <mark>Deterministic function, nondeterministic function errors.</mark>

- Deterministic functions cause errors when used in functions because the database cannot determine the data to be returned.
- That is, a representative deterministic function is the NEWID() function.
- NEWID() is a random function, which means that the database cannot determine the value to return from the function.
- Additionally, try-catch, delete, update, print, etc. also cause errors when used in functions.
- If you understand the principles of non-deterministic functions correctly, you can return them through view instead of calling NEWID() directly.
- This is a reference link for View.
- [How to create and delete an MSSQL view.](https://aurumguide.com/mssql/1007_Eng_view_Create/ "Here is a reference link for View.")

### ***Error when using NEWID().***

- Msg 443, Level 16, State 1, Procedure AurumGuide_DeterministicFunc, Line 10 [Batch Start Line 21]  
    Invalid use of a side-effecting operator 'newid' within a function.
- Source code.

```sql
CREATE FUNCTION dbo.AurumGuide_DeterministicFunc 
(
 @PARAM1 varchar(10)
,@PARAM2 varchar(10)
)
RETURNS varchar(100)
AS
BEGIN
    declare @returnStr varchar(100);
    set @returnStr  = 'Hello World ' +  CAST(NEWID() AS VARCHAR(50));
    RETURN @returnStr
END;
go
```

### ***Error when using try-catch, delete, update, print.***

- Msg 443, Level 16, State 15, Procedure AurumGuide_DeterministicFunc1, Line 16 [Batch Start Line 40]  
    Invalid use of a side-effecting operator 'DELETE' within a function.
- Description of the error.

![This is a description of a non-deterministic function error.](/assets/images/postsImages/MsSql/1057_function_Deterministic/1.png)

- Source code.

```sql
DROP TABLE IF EXISTS AurumGuide_Deterministic;
-- Create Sample Table 
CREATE TABLE AurumGuide_Deterministic (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);
-- Create Sample Data
INSERT INTO dbo.AurumGuide_Deterministic(AurumId,AurumNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian')
      ,(274, N'Stephen')
      ,(275, N'Michael')
      ,(276, N'Linda');
GO      
CREATE FUNCTION AurumGuide_DeterministicFunc1 (@AurumId int)
RETURNS @Return_AurumGuide  TABLE
(  
    AurumId int,
    AurumNm varchar(255) NULL 
) AS
BEGIN
/*
    PRINT(@ParamAurumId);
    DELETE FROM AurumGuide_Deterministic;
    INSERT AurumGuide_Deterministic(AurumId) values(100);
    UPDATE AurumGuide_Deterministic SET AurumId=1;
    SELECT newid();
*/
  IF(@AurumId IS NOT NULL)
  BEGIN
     DELETE FROM AurumGuide_Deterministic;

     INSERT @Return_AurumGuide
     SELECT AurumId,AurumNm
       FROM AurumGuide_Deterministic af
       WHERE af.AurumId  =  @AurumId;
  END 
  RETURN;
END;
```
