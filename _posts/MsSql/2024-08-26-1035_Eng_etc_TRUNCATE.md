---
title: "MSSQL TRUNCATE, DELETE Differences and Usage"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-08-26
last_modified_at: 2024-08-26
---

The TRUNCATE TABLE statement is primarily used to quickly delete all data from an entire table or a specified partition of a table.

## <mark>Difference between TRUNCATE command and DELETE</mark>

### ***Truncate command features.***

- Truncate is a DDL command.
- The TRUNCATE command can delete rows faster than the DELETE command.
- It can initialize the auto-increment column IDENTITY.
- The table structure is not changed. That is, column information, constraints, and indexes are not changed, and only data is deleted.
- It uses less space than the delete statement because it only stores the canceled pages of the table's data storage in the transaction log.
- It cannot use conditional clauses like the where clause.

### ***Features of Delete.***

- Delete is a DML statement.
- It may be slow if there are many rows in the record.
- You can use the condition (where) clause to delete only specific rows.
- You cannot initialize the identity value of an auto-increment column.
- When initializing, use the DBCC CHECKIDENT command.
- The delete statement removes rows one at a time and saves a transaction log for each deleted row, so it requires a lot of capacity compared to Truncate.

## <mark>How to use TRUNCATE.</mark>

### ***Deleting table data using TRUNCATE.***

- Source using TRUNCATE.

```sql
 -- cretable table
DROP TABLE IF EXISTS InfoForTruncate;
CREATE TABLE InfoForTruncate (
  UserId int,
  UserNm varchar(255) 
); 
-- insert sample data 
INSERT INTO dbo.InfoForTruncate(UserId,UserNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian')
      ,(274, N'Stephen')
      ,(275, N'Michael')
     ,(276, N'Linda');
-- check data
SELECT * FROM InfoForTruncate; 
-- excute TRUNCATE  TABLE 
TRUNCATE TABLE InfoForTruncate;
-- check data
SELECT * FROM InfoForTruncate;
```

- This is the TRUNCATE description.

![TRUNCATE, DELETE Differences and Usage](/assets/images/postsImages/MsSql/1035_Eng_etc_TRUNCATE/1.png)

### ***Rollback using TRANSACTION.***

- Source for using TRANSACTION.

```sql
DROP TABLE IF EXISTS TruncateForTran;
CREATE TABLE TruncateForTran ( 
  UserId  int,
  UserNm varchar(255)  
) ON [PRIMARY];

INSERT INTO dbo.TruncateForTran(UserId,UserNm) 
  VALUES (272, N'Ken') ,(273, N'Brian')
;
-- TRANSACTION start
BEGIN TRANSACTION;
TRUNCATE TABLE TruncateForTran;

-- ROLLBACK  check.
ROLLBACK;
SELECT * FROM TruncateForTran;

-- COMMIT  check.
COMMIT;
SELECT * FROM TruncateForTran;
```

### ***Check IDENTITY initialization.***

- IDENTITY initialization source.

```sql
DROP TABLE IF EXISTS TruncateForIDENTITY;
CREATE TABLE TruncateForIDENTITY (
  IdKey   int  IDENTITY (1, 1)  PRIMARY KEY,
  UserId  int,
  UserNm varchar(255)  
) ON [PRIMARY];

INSERT INTO dbo.TruncateForIDENTITY(UserId,UserNm) 
 VALUES (272, N'Ken') ,(273, N'Brian')
 ;

TRUNCATE TABLE InfoForTruncate;

INSERT INTO dbo.TruncateForIDENTITY(UserId,UserNm) 
 VALUES (272, N'Ken') ,(273, N'Brian')
 ;

 -- check data
SELECT * FROM InfoForTruncate;
```

## <mark>Limitations when using TRUNCATE</mark>

- FOREIGN KEY referenced by constraints during TRUNCATE operation.
- You can delete table rows that have foreign keys referencing themselves.
- TRUNCATE operation does not record individual row deletions, so triggers cannot be used.
- TRUNCATE TABLE does not allow EXPLAIN within the statement.
