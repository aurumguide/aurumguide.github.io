---
title: "How to create and delete an MSSQL table"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql/SSMS_Connect/

toc: true
toc_sticky: true
 
date: 2024-07-03
last_modified_at: 2024-07-03
---

Learn how to create and drop tables in the database.
The first is created using SQL Server Management Studio, We will also explain how to use Transact-SQL as a method used by experts, so please try it out.  

## <mark>MS SQL table concept description</mark>

- It's easy to understand if you think of it as a set of data in a database consisting of the structure of table rows and columns.
- Each row represents a record, and each column represents a field. 
- The table is the most basic unit for storing data structurally in a database.
- For example, use table to store student information, grade content, etc.

## <mark>How to create an MS SQL Table</mark>

### How to create a table through SQL Server Management Studio (SSMS)

***1-1. Select Object explorer - Database - Creation Database - Tables - Right mouse - new - Table.***
![Select Object explorer.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/1-1.jpg)

***1-2. TABLE and column input screen.***

- Enter id,name,age,hp_number,write,comment.
- View and enter the Data Type capture screen.
- Allow Nulls report and enter. 
- Click Save when you're done typing.
![Click Save when you're done typing.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/1-2.jpg)

***1-3. Table Name Input Pop-up.***

- Enter the desired name and click OK.
![Enter the desired name and click OK.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/1-3.jpg)

***1-4. Refresh the Object explorer.***

- You can verify that the table has been created.
![You can verify.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/1-4.jpg)

### Create a table using Transact-SQL

***2-1. In the SQL Run window, type the source and click EXE.***

 ```sql
USE [sampleDB]
GO
CREATE TABLE [dbo].[userInfo](
	[id] [nvarchar](50) NOT NULL,
	[name] [nvarchar](50) NULL,
	[age] [smallint] NULL,
	[hp_number] [nvarchar](50) NULL,
	[writeDate] [smalldatetime] NULL,
	[comment] [nvarchar](max) NULL
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO

```

![click EXE.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/2-1.jpg)

***2-2. Check for errors and check the table created by Object explorer.***

![Check for errors.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/2-2.jpg)


### How to rename a table using SSMS.

***3-1. Right mouse - Rename in the table where you want to name Object explorer - Database - Create Database - Tables - Rename.***
![Rename in the table.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/3-1.jpg)

***3-2. If the name of the selected table changes to the modification mode, you can enter the name again and hit Enter to confirm.***
![you can enter the name again and hit Enter.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/3-2.jpg)

## <mark>How to delete MSSQL table</mark>

### Delete table through SQL Server Management Studio (SSMS)

***4-1. Select Object explorer - Database - Create Database - Tables - Right mouse - Delete from the table you want to delete.***
![Select Object explorer.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/4-1.jpg)

***4-2. Click the OK button in the Delete Object window.***
![Delete Object window.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/4-2.jpg)

### Delete table created using Transact-SQL

***5-1. In the SQL Run window, type the source and click Excute.***

```sql  
USE [sampleDB]
GO 
DROP TABLE [dbo].[userInfoFortsql]
GO
```

![click Excute.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/5-1.jpg)

***5-2. Refresh Object explorer. Make sure the table is dropped.***
![the table is dropped.](/assets/images/postsImages/MsSql/1005_Eng_table_Create/5-2.jpg)

## <mark>How to deal with an error</mark>

Msg 3701, Level 11, State 5, Line 6  
Cannot delete table 'dbo.userInfoFortsql1' because it does not exist or does not have permission.

```sql
USE [sampleDB]
GO
IF  EXISTS (SELECT * FROM sys.objects WHERE object_id = OBJECT_ID(N'[dbo].[userInfoFortsql]') AND type in (N'U'))
DROP TABLE [dbo].[userInfoFortsql]
GO
```
