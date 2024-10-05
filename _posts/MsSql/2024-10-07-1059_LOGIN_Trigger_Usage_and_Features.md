---
title: " MSSQL LOGIN Trigger Usage and Features"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-07
last_modified_at: 2024-10-07
---
 
LOGON Trigger is an event that occurs when a user session is established in a SQL Server instance, and is used to control user logins.

## <mark>LOGIN Trigger Characteristics</mark>

- LOGON Trigger can be thought of as another type of stored procedure that responds to the LOGON event.
- LOGON Trigger occurs after the authentication phase of login is completed, but before the user session is established.

- In other words, the trigger is executed when a session is created after authentication of the user ID, password, and authority is completed.
- LOGON Trigger is not executed if authentication of the user ID, password, or authority fails. - LOGON Trigger is used when tracking the activities of logged-in users.
- You can also restrict logins to SQL Server, and limit or control the number of sessions for a specific login.

- See the explanation of the picture in the post about Trigger features and types.

## <mark>How to use LOGIN Trigger</mark>

### ***Creating a LOGIN Trigger.***

- How to restrict user access with LOGIN Trigger sessions.
- This is the source code.

```sql
USE master;
GO
CREATE LOGIN AurumGuideLoginTrigger
WITH PASSWORD = N'aurumguide' MUST_CHANGE,CHECK_EXPIRATION = ON;
GO
GRANT VIEW SERVER STATE TO AurumGuideLoginTrigger;
GO

CREATE TRIGGER tr_AurumGuide_Login_Trigger ON ALL SERVER
WITH EXECUTE AS N'AurumGuideLoginTrigger'
FOR LOGON AS BEGIN
    IF ORIGINAL_LOGIN() = N'AurumGuideLoginTrigger'
    AND (
        SELECT COUNT(*)
        FROM sys.dm_exec_sessions
        WHERE is_user_process = 1
          AND original_login_name = N'AurumGuideLoginTrigger') > 1
    ROLLBACK;
END;
```

- LOGIN Trigger How to set the user's access history.

```sql
USE master;
DROP TABLE IF EXISTS AurumGuide_Login_Trigger;
-- Create Sample Table 
CREATE TABLE AurumGuide_Login_Trigger(
    AurumId           varchar(100),
    loginTime         datetime 
); 
GO 
-- CREATE TRIGGER;
CREATE TRIGGER tr_AurumGuide_Login_Log ON ALL SERVER
WITH EXECUTE AS N'AurumGuideLoginTrigger'
FOR LOGON AS BEGIN
  IF ORIGINAL_LOGIN() = N'AurumGuideLoginTrigger'
  BEGIN 
    insert into AurumGuide_Login_Trigger
    values('AurumGuideLoginTrigger',GETDATE())
     ;
    END;
END;
go
SELECT *
 FROM AurumGuide_Login_Trigger;
```

- AurumGuideLoginTrigger needs permission to insert into the table.
- Error message.
- Logon failed for login 'AurumGuideLoginTrigger' due to trigger execution. Changed database context to 'master'. Changed language setting to us\_english. (Microsoft SQL Server, Error: 17892)
- Please refer to the link for user creation and permissions.
- [How to create and delete MSSQL users.](https://aurumguide.com/mssql/1004_Eng_User_Create/ "How to create and delete MSSQL users.") 

***Source code description.***

![Create a connection history (audit function).](/assets/images/postsImages/MsSql/1059_LOGIN_Trigger_Usage_and_Features/Login_trigger.png)

### ***Delete LOGIN Trigger.***

How to delete LOGIN Trigger?

```sql
DROP TRIGGER tr_AurumGuide_Login_Log ON ALL SERVER;
```

## <mark>LOGIN Trigger Notes</mark>

- When creating a LOGIN Trigger, I want to perform specific tasks for specific LOGIN IDs.
- For example, if the access history for all LOGIN IDs is recorded in a table and the table is accidentally deleted,
- A LOGIN Trigger error occurs and no one can access the server.
- You need to delete the trigger to be able to access SQL Server.
- If it is not possible to resolve, please let me know in the reply.
