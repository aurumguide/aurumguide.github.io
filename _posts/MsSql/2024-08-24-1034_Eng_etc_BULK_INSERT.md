---
title: "How to use MSSQL BULK INSERT and examples"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-08-24
last_modified_at: 2024-08-24
---

BULK INSERT is used to insert large amounts of data files into a database table.

## <mark>Features of BULK INSERT</mark>

- BULK INSERT allows you to insert TXT and CSV files into a table.
- INSERT and ADMINISTER BULK OPERATIONS privileges are required.
- When using a template with BULK INSERT, the maximum number of fields is 1024.
- When entering data, you can divide the work by specifying the BATCHSIZE clause in the BULK INSERT statement.

## <mark>Example of how to use BULK INSERT</mark>

### ***BULK INSERT usage examples and permissions.***

- Constraints are not used by default. To check constraints, use the CHECK_CONSTRAINTS option.
- If triggers are used on the table, the FIRE_TRIGGER option is not specified.
- Use the KEEPIDENTITY option when retrieving ID values ​​from the data file.

### ***Put a txt file into a table.***

- BULK INSERT the contents of a text file into a table.
- ROWTERMINATOR is a row change, and the default row terminator is \\n (line break).
- If an error occurs, change it to '0x0a'.

```sql
-- CREATE TABLE
DROP TABLE IF EXISTS BulkInsertForTxt;
CREATE TABLE BulkInsertForTxt (
    Txt1 varchar(255) NULL,
    Txt2 varchar(255) NULL,
    Txt3 varchar(255) NULL,
    Txt4 varchar(255) NULL
);
 
-- Bulk Insert  
BULK INSERT BulkInsertForTxt 
FROM 'D:\BULK_INSERT_FILE\TEXTFILE.txt'
  WITH
    (
     FIELDTERMINATOR = '|'
    ,ROWTERMINATOR = '\n'
    -- ,ROWTERMINATOR =  '0x0a'
    -- , FIRSTROW = 2
    -- ,LASTROW  =3    
    -- , BATCHSIZE =2
   );
 
-- data check.
SELECT * 
FROM BulkInsertForTxt;
```

![MSSQL BULK INSERT and examples](/assets/images/postsImages/MsSql/1034_Eng_etc_BULK_INSERT/1.png)

### ***Inserting CSV files into tables.***

- You can also BULK INSERT CSV files into tables.

```sql
-- CREATE TABLE
DROP TABLE IF EXISTS BulkInsertForCSV;
CREATE TABLE BulkInsertForCSV (
    CSV1 varchar(255) NULL,
    CSV2 varchar(255) NULL,
    CSV3 varchar(255) NULL,
    CSV4 varchar(255) NULL
);

-- Bulk Insert 
BULK INSERT BulkInsertForCSV 
FROM 'D:\BULK_INSERT_FILE\CSVBULKINSERT.csv'
WITH ( FORMAT = 'CSV'
     -- , FIRSTROW = 2
     -- ,LASTROW  =3    
     -- , BATCHSIZE =2
    , FIELDTERMINATOR = ','
    , ROWTERMINATOR = '0x0a'
   );
 
 SELECT * 
 FROM BulkInsertForCSV
```

## <mark>BULK INSERT Error</mark>

### ***When BULK INSERT error occurs.***

- When BULK INSERT error occurs.
- Msg 4832, Level 16, State 1, Line 13.
- Bulk Load: Unexpected end of file encountered in data file.
- Msg 7399, Level 16, State 1, Line 13.
- An error occurred in the OLE DB provider "BULK" for linked server "(null)". The provider did not provide any information about the error.
- Msg 7330, Level 16, State 2, Line 13.
- Could not fetch a row from OLE DB provider "BULK" for linked server "(null)".
- Please copy and run the source.

```sql
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;

EXEC sp_configure 'Ad Hoc Distributed Queries', 1;
RECONFIGURE;

EXEC SP_CONFIGURE 'Agent XPs', 1;
RECONFIGURE;
```
