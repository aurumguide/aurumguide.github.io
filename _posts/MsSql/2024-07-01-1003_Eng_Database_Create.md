---
title: "Create and delete the MSSQL Server Database"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]
## permalink: /MsSql/Create and delete the MSSQL Server Database/

toc: true
toc_sticky: true
 
date: 2024-07-01
last_modified_at: 2024-07-01
---

Learn how to use SQL Server Management Studio and Transact-SQL on an MSSQL Server to create and delete an MSSQL database.

## <mark>Create and delete an MSSQL the database</mark>

- You can create a database using the UI Tool with SQL Server Management Studio (SSMS).
- There is a way to create a new database using Transact-SQL.
- There is a way to create a command in the CMCD window.
- There is a way to create a database as a backup file.
- You can also delete the database you are using with the UI Tool.
- Transact-SQL can be used to delete the database in use.
- I will explain how to create a database using the UI Tool and Transact-SQL.
 
## <mark>How to create and delete an MSSQL database</mark>

### How to create a database with the UI Tool with SQL Server Management Studio (SSMS)

***1-1. Select Object explorer - click database - right mouse - new database.***
![new database](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/1-1.jpg)

***1-2. In the new Database window - Database name, click the Enter Name - ok button.***
![new Database window](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/1-2.jpg)

***1-3. You can see that a new database has been created in the Object explorer.***
![Object explorer.](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/1-3.jpg)

***1-4. Let me check the location of the file where the new database was created.***

- Select database and right-click - Properties.
![the new database was created.](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/1-4.jpg)

***1-5. Check the path of the file.***
![Check the path of the file.](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/1-5.jpg)

***1-6. Check the MDF, LDF files on my computer through the file explorer.***
![Check the MDF](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/1-6.jpg)

***1-7. Select from the new database and check.***
![the new database](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/1-7.jpg)

### How to delete database with the UI tool with SQL Server Management Studio (SSMS)

- Run SSMS and connect to the SQL Server.
- Please work carefully when deleting the database.

***2-1. Select Object explorer - click database - right mouse - Delete.***
![Select Object explorer](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/2-1.jpg)

***2-2. In the Database Delete window, check Close existing connections, and click the OK button.***
![In the Database Delete window](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/2-2.jpg)


### How to create a new database using Transact-SQL

***3-1. Select a new questionnaire from the standard toolbar.***

- Copy Curry at the bottom and create it. File path can be changed.

``` sql
USE master;
CREATE DATABASE [samDBsql]
 CONTAINMENT = NONE
 ON  PRIMARY 
( NAME = N'samDBsql', FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\DATA\samDBsql.mdf' , SIZE = 8192KB , FILEGROWTH = 65536KB )
 LOG ON 
( NAME = N'samDBsql_log', FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\DATA\samDBsql_log.ldf' , SIZE = 8192KB , FILEGROWTH = 65536KB )
 WITH LEDGER = OFF
 ```

![File path can be changed.](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/3-1.jpg)

***3-2. Click Object explorer - Refresh.***
![Click Object explorer](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/3-2.jpg)

***3-3. You can check the new database.***
![the new database.](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/3-3.jpg)

***3-4. Check the MDF, LDF files on my computer through Windows File Explorer.***
![Check the MDF, LDF files](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/3-4.jpg)


### <mark>How to delete database using Transact-SQL.</mark> 

***4-1.***

```sql
USE [master];

ALTER DATABASE [samDBsql] SET  SINGLE_USER WITH ROLLBACK IMMEDIATE;

DROP DATABASE [samDBsql];
```

![DROP DATABASE](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/4-1.jpg)

***4-2. Confirm database deletion.***

![Confirm database deletion.](/assets/images/postsImages/MsSql/1003_Eng_Database_Create/4-2.jpg)

## <mark>Finish the MS SQL database</mark>

### This is how to deal with errors

Msg 3702, Level 16, State 4, Line 3 database "samDBsql" is currently in use and cannot be deleted.  
🔔 `Answer`: Database cannot be deleted if another session is connected.

``` sql
USE [master];
ALTER DATABASE [samDBsql] SET  SINGLE_USER WITH ROLLBACK IMMEDIATE;
```

### MSSQL database precautions

- Be sure to get a backup before deleting the database.
- There are many ways to back up, but I will post it later.
- It is recommended that you work with T-SQL for data backup and deletion.

