---
title: "How to create, modify, and delete MSSQL AFTER Trigger"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-13
last_modified_at: 2024-10-13
---
 
MSSQL AFTER Trigger is a stored procedure that automatically executes the Trigger after the INSET, UPDATE, DELETE DML statements are executed on the table or view where the Trigger is set.

## <mark>AFTER Trigger Features</mark>

- The Trigger is executed after the INSET, UPDATE, DELETE DML statements are executed on the table or view where the Trigger is set.
- For example, the T-SQL statement of the Trigger is executed after the INSERT statement in the table inputs data to the table.
- Although the user entered data in one table, there is an advantage in that data can be changed in multiple tables using the Trigger.
- When logging in to a website, you can change the login information history and automatically add related tables.

## <mark>How to use AFTER Trigger</mark>

### ***AFTER Trigger creation.***

Trigger delete, insert simultaneous information.

 ![This is a description of the DML Trigger AFTER UPDATE data.](/assets/images/postsImages/MsSql/1062_Trigger_DML_AFTER/dml_after_trigger.jpg)

Trigger AFTER INSERT.

```sql
DROP TABLE IF EXISTS AurumGuide_DML_TR_AFTER_OPTION;
-- Create Sample Table 
CREATE TABLE AurumGuide_DML_TR_AFTER_OPTION (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);
go
 
DROP TABLE IF EXISTS AurumGuide_DML_TR_AFTER_OPTION_LOG; 
CREATE TABLE AurumGuide_DML_TR_AFTER_OPTION_LOG (
    AurumId            INT NOT NULL,
    AurumNm            VARCHAR(255)  NULL,
    AurumEvent         VARCHAR(255)  NULL,
    AurumDateTime      datetime  NULL     
);
go
-- Example: AFTER Trigger
-- DROP TRIGGER dbo.TR_AurumGuide_DML_TR_AFTER_INSERT
CREATE TRIGGER dbo.TR_AurumGuide_DML_TR_AFTER_INSERT
ON dbo.AurumGuide_DML_TR_AFTER_OPTION
AFTER INSERT
AS
INSERT INTO dbo.AurumGuide_DML_TR_AFTER_OPTION_LOG
      (AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'INSERT', GETDATE() FROM inserted;
-- excute INSERT
INSERT INTO dbo.AurumGuide_DML_TR_AFTER_OPTION(AurumId,AurumNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian') ;

-- check Data
SELECT * FROM AurumGuide_DML_TR_AFTER_OPTION;
-- check Data
SELECT * FROM AurumGuide_DML_TR_AFTER_OPTION_LOG;
```

Trigger AFTER UPDATE.

```sql
CREATE TRIGGER dbo.TR_AurumGuide_DML_TR_AFTER_UPDATE
ON dbo.AurumGuide_DML_TR_AFTER_OPTION
AFTER UPDATE
AS
INSERT INTO dbo.AurumGuide_DML_TR_AFTER_OPTION_LOG
      (AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'UPDATE_deleted', GETDATE() 
  FROM deleted
UNION ALL
SELECT AurumId,AurumNm,'UPDATE_inserted', GETDATE() 
  FROM inserted;
    
--update
UPDATE AurumGuide_DML_TR_AFTER_OPTION
   SET AurumId = '373'
       ,AurumNm = 'TR NAME'
  WHERE AurumId = 273;
-- check Data
SELECT * FROM AurumGuide_DML_TR_AFTER_OPTION;
SELECT * FROM AurumGuide_DML_TR_AFTER_OPTION_LOG;
```

Trigger AFTER DELETE.

```sql
CREATE TRIGGER dbo.TR_AurumGuide_DML_TR_AFTER_DELETE
ON dbo.AurumGuide_DML_TR_AFTER_OPTION
AFTER DELETE
AS
INSERT INTO dbo.AurumGuide_DML_TR_AFTER_OPTION_LOG
     (AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'before_DELETE', GETDATE() 
 FROM deleted;
 -- delete
delete
 from AurumGuide_DML_TR_AFTER_OPTION 
 where AurumId = 272;
-- check Data
SELECT * FROM AurumGuide_DML_TR_AFTER_OPTION;
SELECT * FROM AurumGuide_DML_TR_AFTER_OPTION_LOG;
```

### ***Modify AFTER Trigger.***

Modify a Trigger created with the ALTER Trigger command.

```sql
-- ALTER TRIGGER
ALTER TRIGGER dbo.TR_AurumGuide_DML_TR_AFTER_INSERT
ON dbo.AurumGuide_DML_TR_AFTER_OPTION
AFTER INSERT
AS
INSERT INTO dbo.AurumGuide_DML_TR_AFTER_OPTION_LOG
 (AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'INSERT', GETDATE() FROM inserted
UNION ALL
SELECT 100,'AurumGuide','INSERT-ALTER', GETDATE() 
;
-- insert
INSERT INTO dbo.AurumGuide_DML_TR_AFTER_OPTION(AurumId,AurumNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian') ;
-- check Data
SELECT * FROM AurumGuide_DML_TR_AFTER_OPTION;
SELECT * FROM AurumGuide_DML_TR_AFTER_OPTION_LOG;
```

### ***Delete AFTER Trigger.***

Delete the created Trigger.

```sql
-- Delete
DROP TRIGGER dbo.TR_AurumGuide_DML_TR_AFTER_INSERT;
DROP TRIGGER dbo.TR_AurumGuide_DML_TR_AFTER_UPDATE;
DROP TRIGGER dbo.TR_AurumGuide_DML_TR_AFTER_DELETE;
```
