---
title: "How to create and delete MSSQL users"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]
## permalink: /MsSql/SSMS_Connect/

toc: true
toc_sticky: true
 
date: 2024-07-02
last_modified_at: 2024-07-02
---

Let us learn how to create users for the MSSQL database and how to delete users after creation.

## <mark>MSSQL user description</mark>

- You can create and delete users in MSSQL with dba privileges.
- When you create a user, please grant permission after creation according to the purpose of the create user.
- When specifying a password, make it difficult to create it so that there is no security problem.
- Please give ownership rights to users required for database instead of dba rights.
 
## <mark>MSSQL user create, delete</mark>

### How to create through SQL Server Management Studio (SSMS)

- Connect to the SSMS Open Database.  

***1-1. Select Object explorer - Security-Login Click - Right Mouse - new Login.***
![new Login.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/1-1.jpg)

***1-2. Login New Screen.***

- Enter Login name.
- Select SQL Server authentication.
- Enter your password.
- Uncheck the Enforce passworld expiration check box.
- In the Default database, select the sample DB that you created previously.
![Enter Login name.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/1-2.jpg)

***1-3. Login New Screen - Select User Mapping.***

- In Users mapped to this login, select sampleDB.
- Select db_owner in the Database role membership area.
![In Users mapped to this login.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/1-3.jpg)

***1-4. Select the Login New Screen - Status tab.***

- Verify that login - Enabled is selected.
![Enabled is selected.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/1-4.jpg)

**1-5.** Verify that test user is generated.
![Verify that test user is generated.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/1-5.jpg) 

### Created using Transact-SQL

- Access the SSMS Open database.

***2-1. In the SQL Run window, type the source, and click Execute.***
![click Execute.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/2-1.jpg)

***2-2. Check the users created in Object explorer.***
![Check the users created.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/2-2.jpg)

``` sql
USE [master]
GO
CREATE LOGIN [tsqluser] WITH PASSWORD=N'PAAAA', DEFAULT_DATABASE=[sampleDB], CHECK_EXPIRATION=OFF, CHECK_POLICY=ON
GO
use [master];
GO
USE [sampleDB]
GO
CREATE USER [tsqluser] FOR LOGIN [tsqluser]
GO
USE [sampleDB]
GO
ALTER ROLE [db_owner] ADD MEMBER [tsqluser]
GO
```

### Deleting a user through SQL Server Management Studio (SSMS)

***3-1. Object Explorer Screen***

- Select Security - Select the user you created - Right mouse - Delete.
![Object Explorer Screen.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/3-1.jpg)

***3-2. Check the user you created - click the ok button.***

- When prompted, click Yes to completely delete the user.
![Check the user you created.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/3-2.jpg)

### Delete user created using Transact-SQL

***4-1. Select the New Query window.***

- Create a delete source.

![Create a delete source.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/4-1.jpg)

***4-2. Verify that the user created by Security has been deleted.***
![the user created.](/assets/images/postsImages/MsSql/1004_Eng_User_Create/4-2.jpg)

``` sql
USE [master]
GO
DROP LOGIN [tsqluser]
GO
```

## <mark>How to deal with errors</mark>

`Message confirmation.`
Msg 15434, Level 16, State 1, Line 3  
This login cannot be deleted because the user is currently logged in as 'tsqluser'. 

```sql
USE [master]
SELECT session_id
FROM sys.dm_exec_sessions
WHERE login_name = 'tsqluser'
go
KILL 79   
go
USE [master]
GO 
DROP LOGIN [tsqluser]
GO
```

![KILL 79](/assets/images/postsImages/MsSql/1004_Eng_User_Create/5-1.jpg)
