---
title: "How to connect and set up an MSSQL Server (SSMS)"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql]

##permalink: /MsSql/SSMS_Connect/

toc: true
toc_sticky: true
 
date: 2024-06-30
last_modified_at: 2024-06-30
---
In the previous course, we also looked at the latest version of MSSQL installation 2022, as well as the installation of the SSMS (MSSQL Connection Tool).  
In this course, we will find out how to access using SSMS after installing the database.  
<mark>We have taken step-by-step screenshots for anyone to follow</mark>, so please access the database.  
 
## <mark>Connect the MSSQL Server Database</mark>

For the installation process of the SSMS (MSSQL Connection Tool), please refer to the installation process of 2022, the latest version of MSSQL.

### Access to a Windows account

***1. Start on your computer - Browse to SQL Server Management Studio and click on it.***
![SQL Server Management Studio and click on it](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/1.jpg)

***2. Do you see the screen where you can connect to the Connect to Server?***
![you can connect to the Connect to Server](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/2.jpg)

***2-1. Server Type: Choose Database Engine.***
![Choose Database Engine.](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/2-1.jpg)

***2-2. Server name: The name displayed on the screen may vary from computer to computer.***
![Server nam](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/2-2.jpg)

***2-3. Authentication: Please select Window Authentication.***
![Authentication](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/2-3.jpg)

***2-4. Click Connect.***
![Click Connect](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/2-4.jpg)

***3. It is when you connect. Isn't it too easy?***
![It is when you connect](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/3.jpg)

### Access the MSSQL server with an SA account

- When you install MSSQL Server, the SA account is created by default,  but you must change the settings after accessing the Windows account.  
- It is more complicated than accessing it with a Windows account, but do not worry because if you look at the capture screen, you can easily follow it.  

***4. When you log in with a Windows account, click object Explorer - Security - LOGIN - SA - Right mouse***
![When you log in with a Windows account](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/4.jpg)

***4-1. Login Properties Window - General - Please enter your password.***
![Login Properties Window](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/4-1.jpg)

***4-2. Login Properties Window - Status - Click - Select Enabled on the Login Radio button and confirm with the OK button.***
![confirm with the OK](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/4-2.jpg)

***4-3. On the screen you first connected to, select Mouse Server, right-click, and click Properties.***
![select Mouse Server](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/4-3.jpg)

***4-4. Pop-up - SQL Server and Window Authentication mode and click OK.***
![Window Authentication mode](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/4-4.jpg)

**<mark>5. Restart SQL Server.</mark>**

***5-1. Close the SQL Server you connected to, and in Windows, click SQL Server Configuration Manager.***
![click SQL Server Configuration Manager.](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/5-1.jpg)

***5-2. In the window, select SQL Server and right-click. Click -> Restart to restart SQL Server.**
![Restart to restart SQL Server.](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/5-2.jpg)

***5-3. Accessing the MSSQL Server with the SA account.***
![MSSQL Server with the SA account.](/assets/images/postsImages/MsSql/1002_Eng_SSMS_connect/5-3.jpg)

## <mark>How to deal with errors</mark>

- Error case: Microsoft OLE DB Provider for SQL Server error user '80004005' failed to log in because the account 'SA' is currently locked. Your system administrator may unlock it.
- Answer: ALTER LOGIN SA WITH PASSWORD = 'Enter required password' UNLOCK GO command and uncheck 'Force password policy in object Explorer - Security - LOGIN - SA property' and 'Force password expiration'
- Error case: User 'SA' failed to log in (Microsoft SQL Server, Error: 18456).
- Answer: On the gap screen number 4, select the Login Properties window - Status - Click - Login Radio button, click the OK button, reboot, and access.

## <mark>Finish connecting to the MSSQL Server</mark>

- Today, we learned how to access the database after installing it.
- It would be convenient if you could access the Windows account at first and set up the SA account to access it as well.
- You have an SA account, but You cannot log in., so you must reboot the database after setting it up to access it.
