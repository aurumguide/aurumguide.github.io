---
title: "How to create and delete an MSSQL view."
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

## permalink: /MsSql/SSMS_Connect/

toc: true
toc_sticky: true
 
date: 2024-07-05
last_modified_at: 2024-07-05
---

Learn how to create and delete MSSQL View using SQL Server Management Studio and Transact-SQL.

## <mark> Concept of MSSQL View. </mark>

- Data that is processed and displayed by the user by selecting all desired data from one or more tables (or other views) in the relational database.
- The view corresponds to the derivation of the relational model of the relational database, and the MSSQL View is primarily used for the following purposes.
- Data Restriction:` Displays limited access to only data that is allowed to the user.
- `Processing data:` You can refine or change data derived from the original table through a view and show it to the requester.
- `Security:` View allows you to expose only certain columns and hide sensitive data.
- `For example,` sensitive personal information allows you to view data through the MSSQL View.
 
 
## <mark> How to create a View </mark>

### How to create View through SQL Server Management Studio

***1-1. Object explorer - Database- Views - Right-click - select new view.***

![select new view.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/1-1.jpg)

***1-2. Select the table to use for View.***

![Select the table to use for View.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/1-2.jpg)

***1-3. Select a column to use for View.***

![Select a column to use for View.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/1-3.jpg)

***1-4. Enter the name of the View you want to create and click the OK button.***

![click the OK button.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/1-4.jpg)

***1-5. Reload Object explorer.***

![Reload Object explorer.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/1-5.jpg)

### Create View Create View using Transaction.

***2-1. In the SQL Run window, type the source from which you can create a view and click EXCUTE.***

```sql  
USE [sampleDB]
GO
CREATE view [sampleViewTsql] AS
 SELECT [id]  
       ,[name]  
       ,[age]  
       ,[hp_number]  
       ,[writeDate]  
 FROM DBO.userInfo
; 
```

![click EXCUTE.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/2-1.jpg)

***2-2. Check the view created in Object explorer.***

![Check the view created in Object explorer.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/2-2.jpg)

## <mark> How to delete View </mark>

### Delete View through SQL Server Management Studio.

***3-1. Object explorer - Database- Views - right mouse - Delete Select.***

![Delete Select.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/3-1.jpg)

***3-2. Click the OK button in the Delete Object.***

![Click the OK button in the Delete Object.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/3-2.jpg)

### Delete an MSSQL View created using Transact-SQL

***4-1. In the SQL Run window, type the source, and click EXCUTE.***

```sql
USE [sampleDB]
GO 
DROP View [userInfoFortsql]
GO
```

![click EXCUTE.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/4-1.jpg)

***4-2. Refresh in the Object explorer, and make sure that the View has been dropped.***

![Refresh in the Object explorer.](/assets/images/postsImages/MsSql/1007_Eng_view_Create/4-2.jpg)

## <mark> Clean up the MSSQL View </mark>

- Database View may have performance issues, but it is widely used in practice,  
so you need to be familiar with basic knowledge.
- MSSQL View has limitations when you change it using alter view differently from the usual table.
- MSSQL View has restrictions when you insert or delete data.
- MSSQL View restricts direct modification of data to logical areas.
- MSSQL View is often used to show some materials.
