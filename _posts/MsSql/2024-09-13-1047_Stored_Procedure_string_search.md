---
title: "Stored Procedure string search, content search description"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql 

toc: true
toc_sticky: true
 
date: 2024-09-13
last_modified_at: 2024-09-13
---

MSSQL stores the contents and strings of the generated procedure in the database, so you can search the contents of the procedure and the strings using a query or command.

## <mark>Procedure string search, content search description</mark>

- If you know the procedure name, you can search the contents with the sp_helptext command. This is the most frequently used command.
- If you know the object_id, you can search the string contents using OBJECT_DEFINITION.
- You can also search directly in the system view, usually using the TEXT COLUMN of syscomments.
- sys.sql_modules supported by the system view provides all modules defined in the SQL language, so you can also search the procedure contents.
- Of course, triggers, functions, etc. are supported.

![Procedure content search flow chart.](/assets/images/postsImages/MsSql/1047_Stored_Procedure_string_search/1.png)

## <mark>How to search for procedure strings and contents</mark>

### ***How to use sp_helptext command.***

- Use sp_helptext command to search for contents of one procedure.

```sql
DROP TABLE IF EXISTS UserInfoForContextSearch;
CREATE TABLE UserInfoForContextSearch (
    UserId int,
    UserNm varchar(255)   
);

INSERT INTO dbo.UserInfoForContextSearch(UserId,UserNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian')
      ,(274, N'Stephen')
      ,(275, N'Michael')
      ,(276, N'Linda');

 GO
-- DROP PROCEDURE STORE_PROCEDURE_CONTENTS_SEARCH;
CREATE  PROCEDURE [dbo].[STORE_PROCEDURE_CONTENTS_SEARCH]   
    @UserNm NVARCHAR(10) 
AS
BEGIN
   /* Search procedure contents */
    SELECT * 
      FROM UserInfoForContextSearch 
      WHERE UserNm = @UserNm
;
END;

-- sp_helptext command.
EXEC sp_helptext 'STORE_PROCEDURE_CONTENTS_SEARCH';
```

### ***How to use OBJECT_DEFINITION.***

- If you know the OBJECT_ID of the procedure in the SELECT clause, you can search the procedure contents.

```sql
-- How to use OBJECT_DEFINITION.
--example 1
SELECT name AS procedure_name
      ,OBJECT_DEFINITION(object_id)
  FROM sys.procedures 
 WHERE NAME  ='STORE_PROCEDURE_CONTENTS_SEARCH'

--example 2
 SELECT  OBJECT_NAME(object_id)
        ,OBJECT_DEFINITION(object_id)
   FROM sys.objects
  WHERE  OBJECT_NAME(object_id) = 'STORE_PROCEDURE_CONTENTS_SEARCH'
```

### ***Search procedure contents using sys.sql_modules.***

- Mainly used when searching all contents such as procedures, triggers, and functions.

```sql
-- Search procedure contents using sys.sql_modules.
SELECT OBJECT_NAME(OBJECT_ID) AS  PROC_NM
      ,definition             AS  PROC_SORCE
      ,*
FROM sys.sql_modules  
WHERE object_id = (OBJECT_ID(N'dbo.USP_ProcedureSample'));
```

### ***Use the TEXT column of syscomments.***

- syscomments is a system view, and you can search procedure contents through the text column.

```sql
-- Search procedure strings with the TEXT column of syscomments.
SELECT type, name, text
  FROM syscomments S1
  JOIN sys.objects S2 
   ON S1.id = S2.object_id
 WHERE S2.type = 'P'
   AND S1.text LIKE '%ContextSearch%';
```

## <mark>Procedure string search, content search finish</mark>

### ***Trigger, function, view content search method.***

- If necessary, enter a Type condition to search the content.

```sql
 SELECT  TYPE  AS 'OBJECT TYPE' 
        ,TYPE_DESC  AS 'COMMENT'
        ,OBJECT_NAME(object_id) AS 'NAME'
        ,OBJECT_DEFINITION(object_id) AS 'DEFINITION'
        ,*
   FROM sys.objects
 WHERE TYPE_DESC  NOT IN ('SYSTEM_TABLE','INTERNAL_TABLE')
```
