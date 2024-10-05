---
title: "How to use and explain MSSQL Trigger enable, disable"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-23
last_modified_at: 2024-10-23
---

MSSQL Trigger can control the Trigger operation through enable and disable.

## <mark>Trigger enable, disable description.</mark>

- If you need to turn off the Trigger for a while, use the disable command.
- In the case of banks or securities companies, there are cases where the trigger is not executed when a large amount of data is uploaded to the table during regular maintenance time.
- Then, disable the Trigger and start the task.
- After completing the task, you can enable the Trigger again for the service.

## <mark>How to use Trigger enable, disable</mark>

### ***What is the syntax of Trigger enable, disable?***

```sql
-- Syntax
[ENABLE | DISABLE]TRIGGER [Trigger_Name | ALL] ON [Object_Name | DATABASE | ALL SERVER]
```

- **ENABLE:** Used when using stopped triggers.
- **DISABLE:** Used when stopping a trigger that is being used.
- **Trigger Name | ALL:** Used when stopping only one trigger in Trigger_Name, and ALL is used when changing all of them.
- **Object_Name:** Object_Name is usually a table name. Used when changing the trigger status of a specific table.
- **DATABASE:** Trigger status can be changed on a DATABASE basis.
- **ALL SERVER:** Trigger status for the entire server can be changed.

![This is a description of trigger enable and disable.](/assets/images/postsImages/MsSql/1067_Trigger_enable_disable/1.jpg)

### ***Trigger enable, disable Source code.***

```sql
DROP TABLE IF EXISTS AurumGuide_ENABLE;
-- Create Sample Table 
DROP TABLE IF EXISTS AurumGuide_ENABLE; 
CREATE TABLE AurumGuide_ENABLE (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);
go
DROP TABLE IF EXISTS AurumGuide_ENABLE_LOG; 
CREATE TABLE AurumGuide_ENABLE_LOG (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255)  NULL,
    AurumEvent        VARCHAR(255)  NULL,
    AurumDateTime          datetime  NULL     
);
go 
 
CREATE OR ALTER TRIGGER dbo.TR_AurumGuide_ENABLE
ON dbo.AurumGuide_ENABLE
AFTER INSERT
AS
INSERT INTO dbo.AurumGuide_ENABLE_LOG
      (AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'INSERT', GETDATE() FROM inserted;

-- Check status.
SELECT name,
      parent_class_desc,
      type_desc,
      is_disabled
FROM sys.triggers
WHERE name = 'TR_AurumGuide_ENABLE';

-- DISABLE /ENABLE.
-- CASE 1.
DISABLE TRIGGER TR_AurumGuide_ENABLE ON AurumGuide_ENABLE;
ENABLE TRIGGER TR_AurumGuide_ENABLE ON AurumGuide_ENABLE;

-- DISABLE /ENABLE.
-- CASE 2.
ENABLE TRIGGER ALL ON dbo.AurumGuide_ENABLE;
DISABLE TRIGGER ALL ON dbo.AurumGuide_ENABLE;
```
