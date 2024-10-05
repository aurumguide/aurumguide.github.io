---
title: "MSSQL insert, update, delete simultaneous trigger usage"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-17
last_modified_at: 2024-10-17
---

I want to share a trigger that can use insert, update, and delete at the same time in one trigger.

## <mark>Why use a trigger?</mark>

- One of the reasons why triggers are often used is to maintain integrity by saving logs of changes in a table to other tables.
- However, I don't think it's necessary to create a trigger for each insert, update, and delete.
- I think it should be created and managed to a limit for maintenance purposes.
- Of course, there is a disadvantage, and you have to add logic to distinguish the events that occurred.

## <mark>Each event of INSERT, DELETE, and UPDATE</mark>

- INSERT event: INTESTED occurs, DELETED does not occur.
- DELETE event: INTESTED does not occur, DELETED occurs.
- UPDATE event: INTESTED occurs, DELETED occurs.

## <mark>How to use insert, update, and delete simultaneously.</mark>

### ***How to use insert, update, and delete simultaneously.***

![Description of simultaneous insert, update, and delete sources and usage.](/assets/images/postsImages/MsSql/1064_Trigger_INSERT_UPDATE_DELETE/1.png)

### ***Insert, update, delete simultaneous source code.***

```sql
USE sampleDB;
DROP TABLE IF EXISTS AurumGuide_tr_multi;
-- Create Sample Table 
CREATE TABLE AurumGuide_tr_multi (
    AurumId           INT NOT NULL,
    AurumNm           VARCHAR(255) NOT NULL,
    AurumAge          INT  NULL,
    AurumAddress      VARCHAR(500)  NULL
);

DROP TABLE IF EXISTS AurumGuide_tr_multi_log;
CREATE TABLE AurumGuide_tr_multi_log (
    Aurum_id  int identity(1,1) NOT NULL,
    Aurum_Event        VARCHAR(255)   NULL 
);
go
CREATE OR ALTER TRIGGER TR_AurumGuide_multi_process 
ON AurumGuide_tr_multi
FOR INSERT, UPDATE, DELETE
AS  
IF EXISTS (SELECT 1 FROM inserted)
BEGIN
    IF EXISTS (SELECT 1 FROM deleted)
    BEGIN 
        INSERT INTO AurumGuide_tr_multi_log(Aurum_Event)
        VALUES('AurumGuide_UPDATE'); 
    END 
    ELSE
    BEGIN
        INSERT INTO AurumGuide_tr_multi_log(Aurum_Event) 
        VALUES('AurumGuide_INSERT');       
    END
END 
ELSE 
BEGIN
    INSERT INTO AurumGuide_tr_multi_log(Aurum_Event)
    VALUES('AurumGuide_DELETE');   
END;
 
--------------------------------
-- insert 
INSERT INTO dbo.AurumGuide_tr_multi
       (AurumId,AurumNm) 
VALUES (272, N'Ken')
      ,(273, N'Brian');
 
-- update 
update dbo.AurumGuide_tr_multi
    set AurumNm = 'up_AurumNm'
  where AurumId ='272';

-- DELETE 
DELETE 
  FROM  dbo.AurumGuide_tr_multi
WHERE  AurumId ='273';

-- CHECK THE DATA 
SELECT *
FROM AurumGuide_tr_multi_log;
```
