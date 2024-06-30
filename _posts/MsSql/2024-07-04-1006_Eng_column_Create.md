---
title: "How to create and drop an MS SQL column"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql/SSMS_Connect/

toc: true
toc_sticky: true
 
date: 2024-07-04
last_modified_at: 2024-07-04
---

Today, we will learn how to add and drop or rename the created table column.

## <mark> Description of MSSQL column creation.</mark>

- There is a way to create UI through SQL Server Management Studio.
- There is a way to create a database column using Transact-SQL.
- You can also create commands from the CMCD window.
- The database column corresponds to a column in the table.
- It is recommended to back up the column before dropping it.
- You can also drop the database column after renaming it before removing it.

## <mark> How to create an MSSQL column. </mark>

### How to create a table through SQL Server Management Studio (SSMS)

***1-1. Object explorer - Database- Tables - Column -right mouse - Select New Column.***

![Select New Column.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/1-1.jpg)

***1-2. TABLE and column input screen.***

- Enter a “newAddColumn”.
- View and select the Data Type capture screen to enter.
- Select and enter Allow Nulls.
- Click Save when you are done typing.

![TABLE and column input screen.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/1-2.jpg)
***1-3. Refresh the Object explorer.***

- You can verify that the column has been created.

![You can verify.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/1-3.jpg)
 
### Create a column using Transact-SQL

***2-1.  In the SQL Run window, type the source, and click Execute.***

```sql
use [sampleDB]
go
ALTER TABLE dbo.userInfoTsql ADD newAddColumnTsql VARCHAR (25);
```

![click Execute.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/2-1.jpg)

***2-2.  Check for errors and check the post-refresh column created by Object explorer.***

![Check for errors.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/2-2.jpg)

## <mark> How to change and drop the MSSQL column.</mark>

### Delete column through SQL Server Management Studio (SSMS)

***3-1. Object explorer - Database - Tables - Column - Right mouse - Select Delete.***

![Object explorer.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/3-1.jpg)

***3-2. In the Delete Object window, click OK.***

![click OK.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/3-2.jpg)

***3-3. Object explorer - Check for columns after refresh.***

![Check for columns after refresh.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/3-3.jpg)

### Delete a column created using Transact-SQL.

***4-1. In the SQL Run window, type the source, and click Execute.***

 ```sql
use [sampleDB]
go
ALTER TABLE dbo.userInfoTsql DROP COLUMN newAddColumnTsql;
```

![In the SQL Run window, type the source, and click Execute.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/4-1.jpg)

***4-2. Object explorer - Refresh again. Check that the column has been dropped.***

![Check that the column has been dropped.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/4-2.jpg)

### Changes to columns created using Transact-SQL.

***5-1.  In the SQL Run window, type the source, and click Execute.***  

- You can also change the name of the column, but you can also change the size of the data.

![click Execute.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/5-1.jpg)

```sql
use [sampleDB]
go
 
EXEC SP_RENAME 'dbo.userInfoTsql.hp_number', 'cellNumber', 'column';
 
-- DataSize  : VARCHAR(100) -> VARCHAR(200)
ALTER TABLE dbo.userInfoTsql ALTER COLUMN TsqlAddColumnTest VARCHAR(200);
 
-- Datatype : VARCHAR(200) -> NCHAR(500)
ALTER TABLE [table name] ALTER COLUMN [column name] [Type of data to be changed];

ALTER TABLE dbo.userInfoTsql ALTER COLUMN TsqlAddColumnTest NCHAR(500);
 
 ```

***5-2.  Object explorer - Refresh again. Check that the column has been changed.***

![Check that the column has been changed.](/assets/images/postsImages/MsSql/1006_Eng_column_Create/5-2.jpg)

## <mark>Let us wrap it up.</mark>

1. There are many functions to add and change columns, including defaults, not null, constraints, and so on.
2. I will do the added functions when I explain the pk.
