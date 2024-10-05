---
title: "MSSQL DDL Trigger Usage and Features"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-09
last_modified_at: 2024-10-09
---
 
DDL Trigger is mainly used to manage or prevent changes to the schema of SQL SERVER.
For example, if you need the history of table changes, use DDL Trigger.

## <mark>DDL Trigger Features</mark>

- DDL Trigger is used in response to various DDL (Data Definition Language) events.
- DDL Trigger can be used in Transact-SQL statements starting with CREATE, ALTER, DROP, GRANT, DENY, REVOKE.
- DDL Trigger can prevent changes when a specific user attempts to change the database schema.
- DDL Trigger is very useful when managing the history of changes to the database schema.

## <mark>DDL Trigger syntax</mark>

**Trigger syntax.**
```sql
CREATE TRIGGER trigger_name
ON { DATABASE |  ALL SERVER}
[WITH ddl_trigger_option]
FOR { event_type | event_group }
AS 
    { T-sql sentence}
```

### ***Here is the grammar explanation.***

- trigger_name: The name of the trigger being created.
- The ON clause option specifies that the trigger will be executed for database or all server-scoped events.
- ddl_trigger_option can specify Encryption or EXECUTE AS clauses.

- Encryption can encrypt the trigger.
- EXECUTE AS defines the user under which the trigger is executed.
- event_type is the event that the trigger sets, such as CREATE_TABLE, ALTER_TABLE, etc.
- event_group is the group of event_type.

## <mark>How to use DDL Trigger</mark>

### ***Create DDL Trigger.***

**This is a trigger that prevents changes to TABLE.**

```sql
USE sampleDB;
DROP TABLE IF EXISTS AurumGuide_DDL_Trigger;
-- Create Sample Table 
CREATE TABLE AurumGuide_DDL_Trigger(
   AurumId           varchar(100),
   loginTime         datetime 
);
go
-- Do not change TABLE.
CREATE TRIGGER TR_AurumGuide_DDL_Trigger   
ON DATABASE   
FOR DROP_TABLE, ALTER_TABLE   
AS   
   PRINT 'You must disable Trigger "AurumGuide_DDL_Trigger" to drop or alter tables!'   
   ROLLBACK;  
go
-- DROP TABLE
drop table AurumGuide_DDL_Trigger;
```

**Example of saving TABLE change history in table XML format.**

```sql
-- Table change history.
CREATE TABLE dbo.AurumGuide_DDL_Trigger_Log(
   LogID int IDENTITY(1,1) PRIMARY KEY,     
   ChangeId   varchar(100),
   DateChanged datetime,
   EventInfo   xml NULL
);
-- Create a trigger
ALTER TRIGGER TR_AurumGuide_DDL_Trigger_Log
ON DATABASE
FOR
    CREATE_TABLE,
    ALTER_TABLE, 
    DROP_TABLE
AS
BEGIN
   SET NOCOUNT ON;
   INSERT INTO dbo.AurumGuide_DDL_Trigger_Log 
   (ChangeId, DateChanged, EventInfo)
   VALUES 
   (USER,GETDATE(),EVENTDATA());
END;
-- Change table
USE sampleDB;
DROP TABLE IF EXISTS AurumGuide_DDL_Trigger;
-- Create Sample Table 
CREATE TABLE AurumGuide_DDL_Trigger(
   AurumId           varchar(100),
   loginTime         datetime 
);
go
-- Check change history
SELECT *
 FROM AurumGuide_DDL_Trigger_Log
```

**Here is the source code description.**

![This is a description of how to save TABLE change history in table XML format.](/assets/images/postsImages/MsSql/1060_DDL_Trigger_Usage_Features/DDL_Trigger.jpg)

### ***Modify DDL Trigger.***

- You can modify it using ALTER TRIGGER like a stored procedure.

### ***Disable and delete DDL Trigger.***

- This is the source code.

```sql
-- Disable syntax
DISABLE TRIGGER [schema_name.][trigger_name] 
ON [object_name | DATABASE | ALL SERVER]
--  Disabled source code
DISABLE TRIGGER TR_AurumGuide_DDL_Trigger ON DATABASE;
-- delete 
DROP TRIGGER TR_AurumGuide_DDL_Trigger ON DATABASE;
```
