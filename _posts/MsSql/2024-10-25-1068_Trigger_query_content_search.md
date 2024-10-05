---
title: "MSSQL Trigger query and content search"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-25
last_modified_at: 2024-10-25
---
 
Since MSSQL stores the created Trigger name and contents in the database, you can search all information about the created Trigger with a query or command.

## <mark>Trigger search, content search description.</mark>

- MSSQL supports the Trigger search function so that you can check the contents of the created Trigger.
- You can also check the contents using SQL Server Management Studio (SSMS).
- If you know the Trigger name, you can search the contents using the sp_helptext command. This is the most frequently used command.
- If you know the Trigger object_id, you can search the string contents using OBJECT_DEFINITION. - You can also search directly in the system view, usually by using the TEXT COLUMN of syscomments.
- Trigger is supported in the system view sys.sql_modules, which provides all modules defined in the SQL language, so you can also search the contents of the trigger.

**This is a description of the trigger query and content search diagram.**

 ![This is a description of the Trigger lookup and content search image.](/assets/images/postsImages/MsSql/1068_Trigger_query_content_search/1.png)

## <mark>Trigger query, content search method</mark>

### ***How to use sp_helptext command.***

- sp_helptext is the most used command.
- Use sp_helptext command to search the contents of a single trigger.
- This is the source code.

```sql
 -- Create Sample Table 
DROP TABLE IF EXISTS AurumGuide_Text_Search; 
CREATE TABLE AurumGuide_Text_Search (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);
go
DROP TABLE IF EXISTS AurumGuide_Text_Search_Log; 
CREATE TABLE AurumGuide_Text_Search_Log (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255)  NULL,
    AurumEvent        VARCHAR(255)  NULL,
    AurumDateTime          datetime  NULL     
);
go  
CREATE OR ALTER TRIGGER dbo.TR_AurumGuide_Text_Search
ON dbo.AurumGuide_Text_Search
AFTER INSERT
AS
BEGIN
INSERT INTO dbo.AurumGuide_Text_Search_Log
       (AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'INSERT', GETDATE() FROM inserted;
END;

--  1. sp_helptext command.
EXEC sp_helptext 'TR_AurumGuide_Text_Search';
```

#### Use the TEXT column of syscomments.

- syscomments is a system view, and you can search for Trigger content through the text column.
- You can only search for 'TR' in the 'type' condition of the column of sys.objects.

**This is the source code.**

```sql
-- 2. Search for Trigger string with the TEXT column of syscomments.
SELECT type, name, text
 FROM syscomments S1
 JOIN sys.objects S2 
   ON S1.id = S2.object_id
WHERE S2.type IN ('TR')
 AND S1.text LIKE '%TR_AurumGuide_Text_Search%';
```

### ***Search Trigger contents using sys.objects.***

- sys.objects can search almost all contents of Trigger, procedure, and function.
- Use the OBJECT_DEFINITION() function when searching contents.

**This is the source code.**

```sql
-- 3. Search function contents using sys.objects
SELECT OBJECT_NAME(object_id)  FUNC_NAME
      ,OBJECT_DEFINITION(object_id) FUNC_TEXT
 FROM sys.objects
 WHERE  OBJECT_NAME(object_id) = 'TR_AurumGuide_Text_Search'
```

### ***Search Trigger contents using sys.sql_modules.***

- Trigger contents are stored in the definition column of sys.sql_modules.
- Mainly used when searching for all contents such as Trigger, Procedure, Function, etc.
- This is the source code.

```sql
-- 4. Retrieving procedure contents using sys.sql_modules.
SELECT OBJECT_NAME(OBJECT_ID) AS  Trigger_NM
      ,definition             AS  Trigger_SORCE
      ,*
FROM sys.sql_modules  
WHERE object_id = (OBJECT_ID(N'dbo.TR_AurumGuide_Text_Search'));
```
