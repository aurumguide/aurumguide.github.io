---
title: "How to create, modify, and delete MSSQL INSTEAD OF Trigger"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-15
last_modified_at: 2024-10-15
---

If you use the INSTEAD OF Trigger option, when you execute an insert on a table, the insert will not occur on the table in question, but on the table used in the execution statement of the trigger.

## <mark>INSTEAD OF Trigger Features.</mark>

- INSTEAD OF Trigger can be defined as "Instead of Triggers".
- INSTEAD OF Trigger can be defined on a table or view.
- When executing an INSERT statement on a view defined with WITH CHECK OPTION, an SQLE_CHECK_TRIGGER_CONFLICT error will occur. - INSTEAD OF Trigger can change data of view that cannot be inserted, deleted, or modified.
- Only one INSTEAD OF Trigger can be defined in a table.
- INSTEAD OF Trigger cannot use ORDER or WHEN clause.

## <mark>How to use INSTEAD OF Trigger.</mark>

### ***Creating INSTEAD OF Trigger.***

INSTEAD OF INSERT source code

```sql
-- INSTEAD OF Triggers
GO
USE sampleDB;
DROP TABLE IF EXISTS AurumGuide_INSTEAD_OF_TR;
-- Create Sample Table 
CREATE TABLE AurumGuide_INSTEAD_OF_TR (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);
GO

DROP TABLE IF EXISTS AurumGuide_INSTEAD_OF_TR_LOG; 
CREATE TABLE AurumGuide_INSTEAD_OF_TR_LOG (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255)  NULL,
    AurumEvent        VARCHAR(255)  NULL,
    AurumDateTime          datetime  NULL     
);
go
 ----------------------------------------------
 -- INSERT 
 DROP TRIGGER dbo.TR_AurumGuide_INSTEAD_OF_INSERT;
 GO
  -- Example: AFTER Trigger
CREATE TRIGGER dbo.TR_AurumGuide_INSTEAD_OF_INSERT
ON dbo.AurumGuide_INSTEAD_OF_TR
INSTEAD OF INSERT
AS
INSERT INTO dbo.AurumGuide_INSTEAD_OF_TR_LOG
(AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'Inserted', GETDATE() 
FROM Inserted ;

-- insert data
INSERT INTO dbo.AurumGuide_INSTEAD_OF_TR
       (AurumId,AurumNm) 
VALUES (272, N'Ken')
       ,(273, N'Brian')
-- check data
SELECT * FROM AurumGuide_INSTEAD_OF_TR;
SELECT * FROM AurumGuide_INSTEAD_OF_TR_LOG;
```

INSTEAD OF INSERT TRIGGER source, check execution DATA.

![INSTEAD OF INSERT TRIGGER source, check execution DATA.](/assets/images/postsImages/MsSql/1063_Trigger_DML_INSTEADOF/insteadOfTrigger.png)

INSTEAD OF UPDATE Source Code

```sql
 -- UPDATE 
DROP TRIGGER dbo.TR_AurumGuide_INSTEAD_OF_UPDATE;
GO 
CREATE TRIGGER dbo.TR_AurumGuide_INSTEAD_OF_UPDATE
ON dbo.AurumGuide_INSTEAD_OF_TR
INSTEAD OF UPDATE
AS
INSERT INTO dbo.AurumGuide_INSTEAD_OF_TR_LOG
(AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'UPDATE_deleted', GETDATE() 
FROM deleted 
UNION ALL
SELECT AurumId,AurumNm,'UPDATE_Inserted', GETDATE() 
FROM Inserted ;

--update
update AurumGuide_INSTEAD_OF_TR
   set AurumId = '373'
       ,AurumNm = 'TR NAME'
  where AurumId = 273;
-- check data 
SELECT * FROM AurumGuide_INSTEAD_OF_TR;
SELECT * FROM AurumGuide_INSTEAD_OF_TR_LOG;
```

INSTEAD OF DELETE Source Code

```sql
 -- DELETE 
DROP TRIGGER dbo.TR_AurumGuide_INSTEAD_OF_DELETE;
GO 
CREATE TRIGGER dbo.TR_AurumGuide_INSTEAD_OF_DELETE
ON dbo.AurumGuide_INSTEAD_OF_TR
INSTEAD OF DELETE
AS
INSERT INTO dbo.AurumGuide_INSTEAD_OF_TR_LOG
(AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'deleted', GETDATE() 
FROM deleted;

--DELETE
DELETE FROM DBO.AurumGuide_INSTEAD_OF_TR;  
-- Check DELETE table
SELECT * FROM DBO.AurumGuide_INSTEAD_OF_TR;
-- Check the LOG table
SELECT * FROM DBO.AurumGuide_INSTEAD_OF_TR_LOG;
```

### ***Modify INSTEAD OF Trigger.***

Modify a Trigger created with the ALTER Trigger command.

```sql
-- Modify Trigger
ALTER TRIGGER dbo.TR_AurumGuide_INSTEAD_OF_DELETE
ON dbo.AurumGuide_INSTEAD_OF_TR
INSTEAD OF DELETE
AS
-- Modified part
DELETE FROM dbo.AurumGuide_INSTEAD_OF_TR_LOG;

INSERT INTO dbo.AurumGuide_INSTEAD_OF_TR_LOG
(AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'deleted', GETDATE() 
FROM deleted;


--DELETE
DELETE FROM DBO.AurumGuide_INSTEAD_OF_TR;  
-- Check DELETE table
SELECT * FROM DBO.AurumGuide_INSTEAD_OF_TR;
-- Check the LOG table
SELECT * FROM DBO.AurumGuide_INSTEAD_OF_TR_LOG;
```

### ***Delete INSTEAD OF Trigger.***

Delete the created Trigger.

```sql
-- Delete
DROP TRIGGER dbo.TR_AurumGuide_INSTEAD_OF_INSERT;
DROP TRIGGER dbo.TR_AurumGuide_INSTEAD_OF_UPDATE;
DROP TRIGGER dbo.TR_AurumGuide_INSTEAD_OF_DELETE;
```
