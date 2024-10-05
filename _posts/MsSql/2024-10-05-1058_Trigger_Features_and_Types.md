---
title: "MSSQL Trigger Features and Types"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-05
last_modified_at: 2024-10-05
---
 
A trigger, as the name suggests, is a stored procedure that is automatically executed when an event occurs on the database server.

## <mark> MSSQL Trigger Features </mark>

- There are many types of triggers, but in SQL SERVER, LOGON Trigger, DML Trigger, and DDL Trigger are mainly used.
- LOGON Trigger creates a database session when a user logs in.
- That is, Trigger can be used in response to the LOGON event that occurs when a database session is established.
- DML Trigger can be used in response to an event when using the DML (Data Manipulation Language) language.
- DDL Trigger can be used in response to various DDL (Data Definition Language) events that are mainly used.

### ***MSSQL Trigger Description.***

![MSSQL Trigger Features.](/assets/images/postsImages/MsSql/1058_Trigger_Features_and_Types/LOGON_Trigger.png)

## <mark> MSSQL Trigger Type-by-Type Characteristics.</mark>

### ***LOGON Trigger Characteristics.***

- LOGON Trigger is executed in response to the LOGON event that occurs when a user's session is established.

- LOGON Trigger can be created and used in two ways.
- The first way is to create it directly using Transact-SQL statements.
- The second way is to create it as an assembly method in Microsoft. NET Framework CLR (Common Language Runtime) using a C# program and upload it to SQL Server for use.

### ***DDL Trigger Characteristics.***

- DDL Trigger is a trigger that can be used in response to events for DDL operations such as CREATE, ALTER, DROP, etc.
- For example, it is used when a log is required when modifying or deleting a table.

### ***DML Trigger Characteristics.***

- DML Trigger, mainly used by DBAs and developers, is a trigger that can be used in response to events for DML operations such as INSERT, UPDATE, and DELETE.
- In other words, when the data in the table changes, the trigger event is automatically executed.
- DML Trigger is often used to track data logs or apply data integrity.
