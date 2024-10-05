---
title: "How to use MSSQL Trigger Function and its features"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-21
last_modified_at: 2024-10-21
---

In SQL Server, I'm going to introduce special functions that can only be used inside Triggers. Using these functions, you can obtain advanced information that is not easily accessible.

## <mark>What are the supported Trigger Function functions?</mark>

- UPDATE() Function can detect changes to columns.
- COLUMNS_UPDATED() Function is mainly used when checking multiple columns.
- NESTLEVEL() Function is used when checking the nested trigger level.
- EVENTDATA() Function can detect execution information. It is often used in Login Triggers.

 ![This is a description of the Trigger Function.](/assets/images/postsImages/MsSql/1066_Trigger_Function/1.png)

## <mark>Trigger Function Features and Usage.</mark>

### ***The UPDATE() Function.***

- Using UPDATE(), you can find out which columns were changed by the UPDATE or INSERT statement.
- In other words, the UPDATE() Function is very helpful in finding the modified columns.
- The UPDATE() Function accepts only one parameter, which is the column name of the table or view related to the Trigger.
- The UPDATE() Function supports (TRUE = 1; FALSE = 0) so that it can be used in conditional statements such as IF or WHILE.

Here is the source code of the UPDATE() Function.

```sql
-- UPDATE() Function  
DROP TABLE IF EXISTS  AurumGuide_UPDATE_Function; 
CREATE TABLE AurumGuide_UPDATE_Function
(
   AurumId  INT PRIMARY KEY,
   AurumNm   VARCHAR(255)  NULL,
   AurumAge      INT  NULL,
   AurumAddress  VARCHAR(500)  NULL
)
GO
DROP TABLE IF EXISTS  AurumGuide_UPDATE_Function_Log;
CREATE TABLE AurumGuide_UPDATE_Function_Log
(
   AurumGuide_Log varchar(100)
);
GO

INSERT INTO dbo.AurumGuide_UPDATE_Function(AurumId,AurumNm) 
VALUES  (274, N'Stephen')
      ,(275, N'Michael')
      ,(276, N'Linda');
GO

CREATE OR ALTER TRIGGER TR_AurumGuide_UPDATE_Function 
ON dbo.AurumGuide_UPDATE_Function
AFTER UPDATE,INSERT
AS
 
IF UPDATE(AurumNm)
BEGIN
   INSERT INTO AurumGuide_UPDATE_Function_Log
   (AurumGuide_Log) VALUES ('AurumNm was updated');
 
END
 
IF UPDATE(AurumAge)
BEGIN
      INSERT INTO AurumGuide_UPDATE_Function_Log
   (AurumGuide_Log) VALUES ('AurumAge was updated');  
END
 
IF UPDATE(AurumAddress)
BEGIN
   INSERT INTO AurumGuide_UPDATE_Function_Log
   (AurumGuide_Log) VALUES ('AurumAddress was updated');
END
GO

UPDATE dbo.AurumGuide_UPDATE_Function
  SET AurumAge = 100
 WHERE AurumId = 274

  SELECT *
-- DELETE 
  FROM AurumGuide_UPDATE_Function;
  
SELECT *
-- DELETE 
  FROM AurumGuide_UPDATE_Function_Log;
```

### ***The COLUMNS_UPDATED() Function.***

- The UPDATE() function is useful for finding out which columns have changed, but it lacks something when you need to check multiple columns.
- To compensate for this, we are adding support for the COLUMNS_UPDATED() function.
- The COLUMNS_UPDATED function has no parameters and returns a VARBINARY stream that can test multiple columns using a bitmask.

Here is the source code for the COLUMNS_UPDATED() function.

```sql
-- COLUMNS_UPDATED() Function  
GO
DROP TABLE IF EXISTS  AurumGuide_COLUMNS_UPDATED_Function; 
CREATE TABLE AurumGuide_COLUMNS_UPDATED_Function
(
   AurumId  INT PRIMARY KEY,
   AurumNm   VARCHAR(255)  NULL,
   AurumAge      INT  NULL,
   AurumAddress  VARCHAR(500)  NULL
)
GO
 

CREATE OR ALTER TRIGGER TR_AurumGuide_COLUMNS_UPDATED_Function
ON dbo.AurumGuide_COLUMNS_UPDATED_Function
AFTER UPDATE,INSERT 
AS
 
IF COLUMNS_UPDATED() & CAST(0x02 AS int) = 0x02
BEGIN
   PRINT 'AurumNm was changed'
END
 
IF COLUMNS_UPDATED() & CAST(0x04 AS int) = 0x04 
BEGIN
   PRINT 'AurumAge was changed'
END
 
IF COLUMNS_UPDATED() & CAST(0x08 AS int) = 0x08
BEGIN
   PRINT 'AurumAddress was changed'
END
GO

INSERT INTO dbo.AurumGuide_COLUMNS_UPDATED_Function(AurumId,AurumNm) 
VALUES  (274, N'Stephen')
      ,(275, N'Michael')
      ,(276, N'Linda');
GO

UPDATE dbo.AurumGuide_COLUMNS_UPDATED_Function
  SET AurumAge = 100
 WHERE AurumId = 274
 ;
```

### ***The TRIGGER_NESTLEVEL() Function.***

- The TRIGGER_NESTLEVEL() function returns nesting information within DML and DDL Triggers.
- Given the object ID of a Trigger, this function returns the nesting level of that Trigger with the three accepted parameters.
The source code for the TRIGGER_NESTLEVEL() function is:

```sql
-- NESTLEVEL() Function 
DROP TABLE IF EXISTS AurumGuide_Nested_Trigger_1;
-- Create Sample Table 
CREATE TABLE AurumGuide_Nested_Trigger_1 (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);
 
DROP TABLE IF EXISTS AurumGuide_Nested_Trigger_2;
CREATE TABLE AurumGuide_Nested_Trigger_2 (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);

DROP TABLE IF EXISTS AurumGuide_Nested_Trigger_3;
CREATE TABLE AurumGuide_Nested_Trigger_3 (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
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

PRINT  'Nesting level for trigger TR_AurumGuide_Nested_Trigger_2: ' + CAST(TRIGGER_NESTLEVEL(OBJECT_ID('TR_AurumGuide_Nested_Trigger_2', 'TR'))  AS VARCHAR(2));
PRINT  'Number of AFTER Triggers on stack: ' + CAST(TRIGGER_NESTLEVEL(0,'AFTER','DML') AS VARCHAR(2)); 
PRINT  'Trigger Nest Level: ' + CAST(TRIGGER_NESTLEVEL()  AS VARCHAR(2));

END;
go
-- Create Sample Data
INSERT INTO dbo.AurumGuide_Nested_Trigger_1(AurumId,AurumNm) 
VALUES (272, N'Ken')

-- Check Data
SELECT * FROM AurumGuide_Nested_Trigger_1;

SELECT * FROM AurumGuide_Nested_Trigger_2;

SELECT * FROM AurumGuide_Nested_Trigger_3;
```

### ***The EVENTDATA() Function for Triggers.***

- The EVENTDATA() function provides information about the event that executed the DDL or logon trigger.
- Of course, it only works when executed within a trigger context, otherwise it returns NULL.
- It is supported in XML format, and you can check the information by saving it to a file, but it can be used in a table.
- Please see also:
- [MSSQL DDL Trigger Usage and Features.](https://aurumguide.com/mssql/1060_DDL_Trigger_Usage_Features/ "MSSQL DDL Trigger Usage and Features.")

This is the EVENTDATA() function source code.

```sql
-- EVENTDATA() Function 
-- Table change history.
DROP TABLE IF EXISTS AurumGuide_EVENTDATA_Trigger_Log;
CREATE TABLE dbo.AurumGuide_EVENTDATA_Trigger_Log(
    LogID int IDENTITY(1,1) PRIMARY KEY,     
    ChangeId   varchar(100),
    DateChanged datetime,
    EventInfo   xml NULL
);

GO
-- Create a trigger
CREATE OR ALTER TRIGGER TR_AurumGuide_EVENTDATA_Trigger_Log
ON DATABASE
FOR
    CREATE_TABLE,
    ALTER_TABLE, 
    DROP_TABLE
AS
BEGIN
    SET NOCOUNT ON;
    INSERT INTO dbo.AurumGuide_EVENTDATA_Trigger_Log 
    (ChangeId, DateChanged, EventInfo)
    VALUES 
    (USER,GETDATE(),EVENTDATA());
END;
GO
-- Change table
USE sampleDB;
DROP TABLE IF EXISTS AurumGuide_EVENTDATA_Trigger;
-- Create Sample Table 
CREATE TABLE AurumGuide_EVENTDATA_Trigger(
   AurumId           varchar(100),
   loginTime         datetime 
);
go
-- Check change history
SELECT *
 FROM AurumGuide_EVENTDATA_Trigger_Log
```
