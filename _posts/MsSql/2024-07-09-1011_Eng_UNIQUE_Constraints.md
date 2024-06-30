---
title: "How to create and delete MSSQL unique constraints."
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql,Constraints]

## permalink: /MsSql/SSMS_Connect/

toc: true
toc_sticky: true
 
date: 2024-07-09
last_modified_at: 2024-07-09
---
 Setting a unique constraint on a column ensures data integrity for the database by preventing duplicate values from entering certain columns.

## <mark>Unique constraint features</mark>

- `Value uniqueness:` Unique constraints do not allow duplicate values, so you can ensure that you have unique values for specific columns.
- `Allow NULL:` Unique constraints do not allow redundancy, but allow NULL values, but only one NULL value per column.
- `FORIGN KEY REFERENCE:` The unique constraint has the advantage of being referred to as FORIGN KEY.
- `Index creation:` Adding a unique constraint at the time of index creation can automatically check the uniqueness of the value.

## <mark>Create a unique constraint</mark>

***1. How to create a unique automatically in a column while creating a table.***

`Syntax and  Source code.`

```sql
-- Syntax
CREATE TABLE [Table Name] (
  [column1] [dataType] UNIQUE,
  [column2] [dataType],
  [column3] [dataType] 
);
-- Source code. 
USE sampleDB;
DROP TABLE IF EXISTS UserInfoForUnique;
CREATE TABLE UserInfoForUnique (
  ID int UNIQUE,
  LastName varchar(255),
  FirstName varchar(255)    
);
-- Check Error.
insert into UserInfoForUnique(ID,LastName,FirstName)
values(202401,'john','hub');
```

`It's a practice code.`
![Create a unique while creating a table.](/assets/images/postsImages/MsSql/1011_Eng_UNIQUE_Constraints/1.jpg)

***2. Generating UNIQUE constraints on two or more columns.***

 `Syntax and  Source code.`

```sql
-- Syntax
CREATE TABLE [Table Name] (
  [column1] [dataType],
  [column2] [dataType],
  [column3] [dataType],
  CONSTRAINT [Constraint name] UNIQUE([column1,column2]) 
);
-- Source code.   
USE sampleDB;
DROP TABLE IF EXISTS UserInfoForUnique;
CREATE TABLE UserInfoForUnique (
  ID int, 
  LastName varchar(255),
  FirstName varchar(255),
  CONSTRAINT Uk_UserInfoForUnique UNIQUE(ID,FirstName)  
);
```

***3.How to create a UNIQUE constraint on an existing column.***

`Syntax and  Source code.`

```sql
-- Syntax
ALTER TABLE [Table Name] ADD CONSTRAINT [Constraint name] UNIQUE ([column1,column2]);
-- Source code.
USE sampleDB;
DROP TABLE IF EXISTS UserInfoForUnique;
CREATE TABLE UserInfoForUnique (
  ID int, 
  LastName varchar(255),
  FirstName varchar(255) 
); 
ALTER TABLE UserInfoForUnique  ADD CONSTRAINT Uk_UserInfoForUnique UNIQUE (ID,FirstName);
```

***4.How to set up UNIQUE when creating a new column.***
`Syntax and  Source code.`

```sql
-- Syntax.
ALTER TABLE [Table Name] ADD [column1] [dataType] CONSTRAINT [UNIQUE Name] UNIQUE (column1,column2]);
--Source code. 
USE sampleDB;
DROP TABLE IF EXISTS UserInfoForUnique;
CREATE TABLE UserInfoForUnique (
  ID int, 
  LastName varchar(255),
  FirstName varchar(255) 
);

ALTER TABLE UserInfoForUnique ADD addId int  CONSTRAINT Uk_UserInfoForUnique UNIQUE (ID,addId);
```

## <mark>How to search for created unique constraints</mark>

`Syntax and  Source code.`

```sql
-- Syntax
EXEC SP_HELP[Table Name];
-- Source code
EXEC SP_HELP UserInfoForUnique;
-- Syntax code
select CONSTRAINT_CATALOG
      ,CONSTRAINT_SCHEMA
      ,CONSTRAINT_NAME
      ,TABLE_CATALOG
      ,TABLE_SCHEMA
      ,TABLE_NAME
      ,COLUMN_NAME
      ,ORDINAL_POSITION
from INFORMATION_SCHEMA.KEY_COLUMN_USAGE
where TABLE_NAME  = 'Table Name';

-- Source code 
USE sampleDB;
DROP TABLE IF EXISTS UserInfoForUnique;
CREATE TABLE UserInfoForUnique (
      ID int unique, 
      LastName varchar(255),
      FirstName varchar(255) 
);

select CONSTRAINT_CATALOG
      ,CONSTRAINT_SCHEMA
      ,CONSTRAINT_NAME
      ,TABLE_CATALOG
      ,TABLE_SCHEMA
      ,TABLE_NAME
      ,COLUMN_NAME
      ,ORDINAL_POSITION
from INFORMATION_SCHEMA.KEY_COLUMN_USAGE
where TABLE_NAME  = 'UserInfoForUnique' ; 

```

`It's a practice code.`

![unique search in the schema.](/assets/images/postsImages/MsSql/1011_Eng_UNIQUE_Constraints/2.jpg)


## <mark>Drop created unique constraints</mark>

`Syntax and  Source code.`

```sql
-- drop Syntax.
ALTER TABLE [Table Name] DROP CONSTRAINT [UNIQUE Name]
 
-- Source code.
USE sampleDB;
DROP TABLE IF EXISTS UserInfoForUnique;
CREATE TABLE UserInfoForUnique (
  ID int, 
  LastName varchar(255),
  FirstName varchar(255),
  CONSTRAINT Uk_UserInfoForUniqueDrop UNIQUE(ID,FirstName)  
);
ALTER TABLE UserInfoForUnique  DROP CONSTRAINT Uk_UserInfoForUniqueDrop;
```
