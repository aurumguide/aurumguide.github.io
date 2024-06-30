---
title: "Concepts for MSSQL FORIGN KEY and how to add, delete, and change Fk."
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql,Constraints]

## permalink: /MsSql/SSMS_Connect/

toc: true
toc_sticky: true
 
date: 2024-07-07
last_modified_at: 2024-07-07
---

We will discuss foreign keys, which are important concepts in relational databases, and practice how to generate them in foreign KEY tables in MSSQL.

## <mark> Concept of FORIGN KEY</mark>

- Foreign Key is a very important concept in relational databases. 
- Foreign Key refers to a key that identifies the rows of one table of fields in another table.
- To put it simply, the foreign key acts as a link to connect relationships between tables.
- FORIGN KEY must be designed identically for the child column and for the parent column.
- If you refer to a parent column in a child column, design the parent column as a unique column.
- Parent columns cannot be deleted when referenced in Child columns.
- If you are using FORIGN KEY, we recommend the code type.
- In relational databases, FORIGN KEY is a very important concept for maintaining data integrity.

## <mark>How to create FORIGN KEY</mark>

***1. How to generate FORIGN KEY in a column when creating a table.***

`Syntax and  Source code.`

```sql
-- Syntax.
CREATE TABLE [TABLE NAME] (
  [COLUMN1] [dataType],
  [COLUMN2] [dataType],
  [COLUMN3] [dataType] FOREIGN KEY REFERENCES [Reference Table Name] ([Reference column name]),
  [COLUMN4] [dataType],
...
)
go
-- Source code. 
DROP TABLE IF EXISTS UserInfoForFk;
CREATE TABLE UserInfoForFk (
  userId      varchar(20) PRIMARY KEY,
  userNames   varchar(50),
  userHp      int,
  userAddress nvarchar(200)
);

DROP TABLE IF EXISTS boardForFk;
CREATE TABLE boardForFk (
  boardId       int,
  boardTitle    nvarchar(100),
  writeId       varchar(20) FOREIGN KEY REFERENCES UserInfoForFk (userId),
  writeDt       datetime
);
```

`Results of executing source code.`
![How to generate FORIGN KEY in a column when creating a table.](/assets/images/postsImages/MsSql/1009_Eng_FK_Constraints/1-1.jpg)

 
***2. How to create a table by specifying the FORIGN KEY name.***

`Syntax and  Source code.`

```sql
-- Syntax.  
CREATE TABLE [TABLE NAME] (
  [COLUMN1] [dataType] NOT NULL,
  [COLUMN2] [dataType],
  [COLUMN3] [dataType],
  [COLUMN4] [dataType], 
CONSTRAINT [FOREIGN KEY명] FOREIGN KEY (COLUMN3)
REFERENCES [Reference Table Name] ([Reference column name])
);
-- Source code.
USE sampleDB;
DROP TABLE IF EXISTS UserInfoForFk;
CREATE TABLE UserInfoForFk (
  userId      varchar(20) PRIMARY KEY,
  userNames   varchar(50),
  userHp      int,
  userAddress nvarchar(200)
);

DROP TABLE IF EXISTS boardForFk;
CREATE TABLE boardForFk (
  boardId     int,
  boardTitle  nvarchar(100),
  writeId     varchar(20),
  writeDt     datetime,
CONSTRAINT Fk_UserInfoForFk  FOREIGN KEY(writeId)  REFERENCES UserInfoForFk (userId)
);
```

`Results of executing source code.`
![How to create a table by specifying the FORIGN KEY name.](/assets/images/postsImages/MsSql/1009_Eng_FK_Constraints/1-2.jpg)

***3. How to generate FORIGN KEY after creating a table.***

`Syntax and  Source code.`

```sql
-- syntax. 
ALTER TABLE [TABLE NAME] ADD CONSTRAINT [FOREIGN KEY명] 
FOREIGN KEY ([column name]) REFERENCES [Reference Table Name] ([Reference column name]) 

-- Source code.
ALTER TABLE boardForFk  ADD CONSTRAINT Fk_UserInfoForFk01
FOREIGN KEY (writeId) REFERENCES UserInfoForFk (userId);
```

`Results of executing source code.`
![How to generate FORIGN KEY after creating a table.](/assets/images/postsImages/MsSql/1009_Eng_FK_Constraints/1-3.jpg)

## <mark>How to enter and delete data with FORIGN KEY</mark>

***1. This is the order of FORIGN KEY INSERT in the table.***

- If you enter the parent table and the child table in that order, no errors will occur.

`It is a practice code.`

```sql
-- If you enter child table first when there is no parent table data.	　　
INSERT INTO DBO.boardForFk(boardId,boardTitle,writeId,writeDt)
values ('1',N'aurumGuideName','aurumguide',getdate())
go
-- If you type in order.
INSERT INTO DBO.UserInfoForFk(userId,userNames,userHp,userAddress)
values ('aurumguide',N'aurumGuideName',0101191140,'seoul')
go
INSERT INTO DBO.boardForFk(boardId,boardTitle,writeId,writeDt)
values ('1',N'aurumGuideName','aurumguide',getdate())
go
```

`Results of executing source code.`
![This is the order of FORIGN KEY INSERT in the table.](/assets/images/postsImages/MsSql/1009_Eng_FK_Constraints/2-1.jpg)

***2. How to delete FORIGN KEY associated data.***

- You must delete the child table in reverse the order of input and delete the parent table.

`It is a practice code.`

```sql
-- If you delete the parent table first
delete from DBO.UserInfoForFk  where userId ='aurumguide'

-- If you delete the child table first and then delete the parent table.
delete from DBO.boardForFk  where writeId ='aurumguide';
go
delete from DBO.UserInfoForFk  where userId ='aurumguide';
```

`Results of executing source code.`
![How to delete FORIGN KEY associated data.](/assets/images/postsImages/MsSql/1009_Eng_FK_Constraints/2-2.jpg)

***3. How to DROP FORIGN KEY.***

`It is a practice code.`

```sql
-- Syntax. 
ALTER TABLE [table Name] DROP CONSTRAINT [FOREIGN KEY Name]
-- It's a practice code.
-- Fk DROP
ALTER TABLE DBO.boardForFk DROP CONSTRAINT Fk_UserInfoForFk01

```

## <mark> How to search FOREIGN KEY in a table </mark>

`Syntax and  Source code.`

```sql
-- Syntax. 
 SP_HELPCONSTRAINT [table Name];

-- It's a practice code.
    SP_HELPCONSTRAINT boardForFk;
```

`Results of executing source code.`
![How to search FOREIGN KEY in a table.](/assets/images/postsImages/MsSql/1009_Eng_FK_Constraints/3.jpg)
