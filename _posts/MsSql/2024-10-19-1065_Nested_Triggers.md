---
title: "How to Use MSSQL Nested Triggers"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-19
last_modified_at: 2024-10-19
---

Nested Triggers are structures where the Trigger of the first table inserts the second table, and the Trigger of the second table inserts the third table.

## <mark>Description of Nested Triggers.</mark>

- Nested Triggers are easier to talk about if you think of them as a structure where Triggers call Triggers.
- Nested Triggers in SQL Server are broadly divided into two types: AFTER Trigger and INSTEAD OF Trigger.
- SQL Server supports up to 32 Nested Triggers for DML and DDL Triggers.
- If you run Nested Triggers in an infinite loop, the nesting level will be exceeded and the Trigger will be terminated. - Nested Triggers in SQL Server are Triggers that are executed as a result of another Trigger execution.

**Description of Nested Triggers.**

![Here's a description of Nested Triggers.](/assets/images/postsImages/MsSql/1065_Nested_Triggers/1.png)

## <mark>How to use Nested Triggers.</mark>

**Nested Triggers source code.**

```sql
DROP TABLE IF EXISTS AurumGuide_Nested_Trigger_1;
-- Create Sample Table 
CREATE TABLE AurumGuide_Nested_Trigger_1 (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);
 
DROP TABLE IF EXISTS AurumGuide_Nested_Trigger_2;
CREATE TABLE AurumGuide_Nested_Trigger_2 (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);

DROP TABLE IF EXISTS AurumGuide_Nested_Trigger_3;
CREATE TABLE AurumGuide_Nested_Trigger_3 (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);

DROP TABLE IF EXISTS AurumGuide_Nested_Trigger_4;
CREATE TABLE AurumGuide_Nested_Trigger_4 (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);
 
--Trigger 
GO
CREATE OR ALTER TRIGGER TR_AurumGuide_Nested_Trigger_1
ON AurumGuide_Nested_Trigger_1
FOR INSERT
AS  
BEGIN
  INSERT INTO AurumGuide_Nested_Trigger_2 
  SELECT * FROM inserted; 
END;
 
GO

 CREATE OR ALTER TRIGGER TR_AurumGuide_Nested_Trigger_2
ON AurumGuide_Nested_Trigger_2
FOR INSERT
AS  
BEGIN
  INSERT INTO AurumGuide_Nested_Trigger_3
  SELECT * FROM inserted; 
END;

go

CREATE OR ALTER TRIGGER TR_AurumGuide_Nested_Trigger_3
ON AurumGuide_Nested_Trigger_3
FOR INSERT
AS  
BEGIN
  INSERT INTO AurumGuide_Nested_Trigger_4
  SELECT * FROM inserted; 
END;

-- Create Sample Data
INSERT INTO dbo.AurumGuide_Nested_Trigger_1(AurumId,AurumNm) 
VALUES (272, N'Ken')

-- Check Data
SELECT * FROM AurumGuide_Nested_Trigger_1;

SELECT * FROM AurumGuide_Nested_Trigger_2;

SELECT * FROM AurumGuide_Nested_Trigger_3;

SELECT * FROM AurumGuide_Nested_Trigger_4;
```
