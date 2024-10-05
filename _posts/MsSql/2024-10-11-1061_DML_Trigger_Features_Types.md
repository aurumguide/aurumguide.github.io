---
title: "MSSQL DML Trigger Features and Types"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-11
last_modified_at: 2024-10-11
---

DML Trigger is a stored procedure that can automatically check the data that is changed when a change occurs in a table or view.

## <mark>DML Trigger Features</mark>

- DML events include INSERT, UPDATE, or DELETE statements.
- You can use DML Triggers to enforce business rules and data integrity, query other tables, and include complex Transact-SQL statements.
- You can apply restrictions to INSERT, UPDATE, and DELETE statements that are more complex than those defined by CHECK constraints.
- DML Triggers can check the state of a table before and after data modification and perform completely different tasks depending on the differences. - If you create multiple DML Triggers on a single table, you can execute different actions when the same modification statement is executed.
- DML Triggers can analyze changes that violate referential integrity by disallowing them or storing the data that is rolled back in another table.

## <mark>DML Trigger syntax.</mark>

### ***Syntax.***

```sql
CREATE TRIGGER [schema_name.]trigger_name
ON { table_name | view_name }
{ AFTER | INSTEAD OF } {[INSERT],[UPDATE],[DELETE]}
[NOT FOR REPLICATION]
AS
    {Tsql- sentence}
```

### ***Syntax Description.***

- schema_name specifies the schema. The default is DBO.
- trigger_name specifies the name of the trigger to be created.
- ON { table_name | view_name } is the name of the table or view on which the DML trigger is created.
- AFTER clause specifies the INSERT, UPDATE, or DELETE event that will fire the trigger.

- AFTER clause is an option that causes the trigger to fire only after SQL Server successfully completes executing a DML statement.
- INSTEAD OF clause is used to skip the INSERT, UPDATE, or DELETE statement to the table and instead execute another statement defined in the trigger. - So no actual INSERT, UPDATE, or DELETE statements are fired at all.
- The [NOT FOR REPLICATION] clause is an option that tells SQL Server not to call the Trigger when the REPLICATION agent modifies the table.
- The {Tsql- statement} part is the T-SQL statement that will be executed in the Trigger when the event occurs.

## <mark>DML Trigger Type</mark>

### ***AFTER Trigger.***

AFTER Trigger is executed after a Trigger action such as insert, update, or delete.

Source code.

```sql
--- AFTER TRIGGER source code.
USE sampleDB;
DROP TABLE IF EXISTS AurumGuide_DML_AFTER_TR;
-- Create Sample Table 
CREATE TABLE AurumGuide_DML_AFTER_TR (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);
-- Create Sample Data
INSERT INTO dbo.AurumGuide_DML_AFTER_TR(AurumId,AurumNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian')
      ,(274, N'Stephen')
      ,(275, N'Michael')
      ,(276, N'Linda');

DROP TABLE IF EXISTS AurumGuide_DML_AFTER_TR_LOG; 
CREATE TABLE AurumGuide_DML_AFTER_TR_LOG (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255)  NULL,
    AurumEvent        VARCHAR(255)  NULL,
    AurumDateTime          datetime  NULL     
);
 go
-- Example: AFTER Trigger
-- DROP TRIGGER dbo.TR_AurumGuide_DML_AFTER
CREATE TRIGGER dbo.TR_AurumGuide_DML_AFTER
ON dbo.AurumGuide_DML_AFTER_TR
AFTER UPDATE
AS
INSERT INTO dbo.AurumGuide_DML_AFTER_TR_LOG
     (AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'UPDATE', GETDATE() FROM DELETED;
go
--update
update AurumGuide_DML_AFTER_TR
  set AurumId = '373'
     ,AurumNm = 'TR NAME'
  where AurumId = 273;

--  Check the table UPDATE
SELECT * FROM AurumGuide_DML_AFTER_TR;
-- Check the LOG table
SELECT * FROM  AurumGuide_DML_AFTER_TR_LOG;
```

Here is the source description.
![Here is the AFTER TRIGGER source code explanation.](/assets/images/postsImages/MsSql/1061_DML_Trigger_Features_Types/1.png)

### ***INSTEAD OF Trigger.***

- INSTEAD OF Trigger is executed instead of the table in T-SQL that the Trigger is specified for.

- For example, if there is an INSTEAD OF UPDATE Trigger on the AurumGuide table and an UPDATE statement is executed for the AurumGuide table, the UPDATE statement will not change the rows in the AurumGuide table.

- That is, if you use an UPDATE statement, the INSTEAD OF UPDATE Trigger will be executed, causing changes to the AurumGuide_LOG table specified in T-SQL.

Source code.

```sql
-- INSTEAD OF Triggers
GO
USE sampleDB;
DROP TABLE IF EXISTS AurumGuide_DML_INSTEAD_TR;
-- Create Sample Table 
CREATE TABLE AurumGuide_DML_INSTEAD_TR (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);
GO
-- Create Sample Data
INSERT INTO dbo.AurumGuide_DML_INSTEAD_TR(AurumId,AurumNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian')
      ,(274, N'Stephen')
      ,(275, N'Michael')
      ,(276, N'Linda');
GO
DROP TABLE IF EXISTS AurumGuide_DML_INSTEAD_TR_LOG; 
CREATE TABLE AurumGuide_DML_INSTEAD_TR_LOG (
  AurumId           INT NOT NULL,
  AurumNm           VARCHAR(255)  NULL,
  AurumEvent        VARCHAR(255)  NULL,
  AurumDateTime     datetime  NULL     
);

 go
 DROP TRIGGER dbo.TR_AurumGuide_DML_INSTEAD;
 GO
 -- Example: AFTER Trigger
CREATE TRIGGER dbo.TR_AurumGuide_DML_INSTEAD
ON dbo.AurumGuide_DML_INSTEAD_TR
INSTEAD OF DELETE
AS
INSERT INTO dbo.AurumGuide_DML_INSTEAD_TR_LOG
(AurumId,AurumNm, AurumEvent, AurumDateTime)
SELECT AurumId,AurumNm,'UPDATE', GETDATE() FROM DELETED;

--DELETE
DELETE FROM DBO.AurumGuide_DML_INSTEAD_TR;  

--Check the DELETE 
SELECT * FROM DBO.AurumGuide_DML_INSTEAD_TR;

-- Check the LOG  
SELECT * FROM DBO.AurumGuide_DML_INSTEAD_TR_LOG;
```

Source description.
![Here is the explanation of INSTEAD OF Triggers.](/assets/images/postsImages/MsSql/1061_DML_Trigger_Features_Types/2.png)

### ***FOR Trigger.***

- I will briefly explain it to developers and DBAs who ask about FOR Trigger.
- To conclude, FOR Trigger is no different from AFTER Trigger.
- Before SQL Server 2000, FOR Trigger was the only type of Trigger that could be used, and it is still in the syntax.
- If you are not a user of a version prior to SQL Server 2000, you can use AFTER Trigger.