---
title: "Description, addition, and deletion of PRIMARY KEY"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql,Constraints]
## permalink: /MsSql/SSMS_Connect/

toc: true
toc_sticky: true
 
date: 2024-07-06
last_modified_at: 2024-07-06
---

Today, we will learn how to add and delete or rename the created table column.

## <mark>Features for PRIMARY KEY</mark>

- PK is a column or combination of columns with values that uniquely identify each row in the table. 
- For example, the ID of the resident registration number, student number, employee number, and Naver is pk created. 
- The primary key must be unique and non-empty.
- Most typically, the identity of the table is created as a unique PK, ensuring integrity for the ID.
- The default key can be generated automatically or customized.
- The constraints of PKs apply data uniqueness and integrity by automatically generating a unique index for the database PK columns.
- Only one constraint in a PK can be created in the table, and all PK columns must be defined as NOT NULL.

## <mark>Create PRIMARY KEY</mark>

- The PRIMARY KEY name must be created in accordance with the naming regulations set out by each project.
- PRIMARY KEY cannot be modified, but can only be created or deleted, so when you create a new one, delete the existing PRIMARY KEY and create it again.
- It is practically impossible to erase and recreate PRIMARY KEY when operating, so you have to make it with care.

***1. How to automatically generate PRIMARY KEY when creating a table.***

```sql
-- Syntex
CREATE TABLE [TABLE Name] (
    column1  Datatype PRIMARY KEY,
    column2  Datatype,
    ...
);
-- Source code.
USE sampleDB;
DROP TABLE IF EXISTS pkUserInfoAuto;
CREATE TABLE pkUserInfoAuto (
id varchar(20) PRIMARY KEY,
names varchar(50)
);
```

`Source code execution screen.`
![How to automatically generate PRIMARY KEY when creating a table.](/assets/images/postsImages/MsSql/1008_Eng_pk_Constraints/1.jpg)

***2. Create PRIMARY KEY and name at the same time when creating a table.***
`Source code 2-1.`

```sql
-- 2-1. Syntex
CREATE TABLE [TABLE Name] (
  column1  Datatype constraint [PK Name] PRIMARY KEY,
  column2  Datatype,
	...
); 
-- Source code.  
USE sampleDB;
CREATE TABLE pkUserInfoNamed1 (
  id varchar(20) constraint  pk_pkUserInfoNamed1 PRIMARY KEY,
  names varchar(50)
);
```

`Source code execution screen.`
![Create PRIMARY KEY Case1.](/assets/images/postsImages/MsSql/1008_Eng_pk_Constraints/2-1.jpg)

`Source code 2-2.`

```sql
-- 2-2.  Syntex 
CREATE TABLE [TABLE Name] (
    column1  Datatype,
    column2  Datatype,
    ...,
	constraint  [PK Name]  PRIMARY KEY(column1,column2,...)
); 

-- Source code.
CREATE TABLE pkUserInfoNamed2 (
  id varchar(20) ,
  names varchar(50),
  constraint  pk_pkUserInfoNamed2  PRIMARY KEY(id,names)
);
```

`Source code execution screen.`
![Create PRIMARY KEY case2.](/assets/images/postsImages/MsSql/1008_Eng_pk_Constraints/2-2.jpg)

***3. How to add PRIMARY KEY after creating a table.***

`Source code.`

```sql
-- Syntex
ALTER TABLE [TABLE Name]
ADD CONSTRAINT [PK constraint name] PRIMARY KEY (column1, column2, column3, ...);

-- Source code. 
USE sampleDB;
CREATE TABLE userInfoPkAfterTable (
  id varchar(20) not null ,
  names varchar(50) not null,
  addcol varchar(100)
);

-- CREATE PK  
ALTER TABLE userInfoPkAfterTable
ADD CONSTRAINT PK_userInfoPkAfterTable PRIMARY KEY (id, names);
```

`Source code execution screen.`
![How to add PRIMARY KEY after creating a table.](/assets/images/postsImages/MsSql/1008_Eng_pk_Constraints/3.jpg)

***4. Set NOT NULL properties when adding columns.***  
`Source code.`

```sql
-- Syntex
ALTER TABLE [TABLE Name]
ALTER COLUMN [Column Name] VARCHAR(10) NOT NULL;

-- Source code. 
USE sampleDB;
CREATE TABLE userInfoPkAfterTable (
  id varchar(20)  ,
  names varchar(50)   
);

ALTER TABLE dbo.userInfoPkAfterTable
ALTER COLUMN id VARCHAR(10) NOT NULL;

ALTER TABLE dbo.userInfoPkAfterTable
ALTER COLUMN names VARCHAR(10) NOT NULL;
```

`Source code execution screen.`
![Set NOT NULL properties when adding columns.](/assets/images/postsImages/MsSql/1008_Eng_pk_Constraints/4.jpg)

***5. Verify PRIMARY KEY creation.***

`Source code.`

```sql
SELECT  * Column Information.
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE  
WHERE table_name = '[TABLE Name]';

USE sampleDB;
SELECT CONSTRAINT_CATALOG
      ,CONSTRAINT_SCHEMA
      ,CONSTRAINT_NAME
      ,TABLE_CATALOG
      ,TABLE_SCHEMA
      ,TABLE_NAME
      ,COLUMN_NAME
      ,ORDINAL_POSITION
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE 
WHERE table_name = 'userInfoPkAfterTable'
```

`Source code execution screen.`
![Verify PRIMARY KEY creation.](/assets/images/postsImages/MsSql/1008_Eng_pk_Constraints/5.jpg)

***6.  DROP PRIMARY KEY.***  
`Source code.`

```sql
-- Syntex
ALTER TABLE [TABLE Name]
DROP CONSTRAINT [PK Constraint Name];
 
-- Source code. DROP PK 
USE sampleDB;
ALTER TABLE  userInfoPkAfterTable DROP CONSTRAINT pk_userInfoPkAfterTable
```

`Source code execution screen.`
![DROP PRIMARY KEY.](/assets/images/postsImages/MsSql/1008_Eng_pk_Constraints/6.jpg)
