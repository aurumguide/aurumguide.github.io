---
title: "Summary of MSSQL CONVERT function usage and performance degradation"
excerpt: ""

categories:
  - MsSql
tags:
  - [MSSQL FUNCTION]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-08-14
last_modified_at: 2024-08-14
---

  
 
The CONVERT function supported in MSSQL is a function used when converting data types.

## <mark>Features of the CONVERT function</mark>

- The CONVERT function is used to convert data from one type to another.
- It is useful when converting a string to a number or converting a date format to another format.
- You can express datetime in various formats using the date output format table.

## <mark>How to use and explain the CONVERT function</mark>

### ***Syntax of the CONVERT function***

```sql
CONVERT ( data_type [( length )] , expression, [style])
```

- data_type: The data type to convert the data to.
- length: The length of the target data type.
- expression: The actual data or field to convert.
- style: This is optional and is mainly used to determine the output format when converting date and time data types.

### ***Change data TYPE.***

- You can return the data entered in the table in various ways by converting it. - An error occurs when converting character data to SMALLINT or INT.

```sql
-- UserInfoForConvert
DROP TABLE IF EXISTS UserInfoForConvert;
CREATE TABLE dbo.UserInfoForConvert
(
    EmpID SMALLINT, 
    EmpName NVARCHAR(40), 
    InputDate VARCHAR(8),
    DeptID INT,
    WriteDate datetime
); 

INSERT INTO UserInfoForConvert VALUES 
(110,'Jessica','20240101','10',getdate()),
(111,'Alicia','20240301','10',getdate()),
(120,'Michael','20240401','20',getdate()),
(121,'Alicia','20240914','20',getdate()) ;
--   example
SELECT CONVERT(NVARCHAR(10),EmpID) AS EmpID FROM UserInfoForConvert --Convert to  VARCHAR로  
SELECT CONVERT(VARCHAR(1),EmpName) AS EmpName FROM UserInfoForConvert --Convert to 1-digit VARCHAR
SELECT CONVERT(CHAR,InputDate,121) AS InputDate FROM UserInfoForConvert --Convert to  CHAR 
SELECT CONVERT(SMALLINT,DeptID) AS DeptID FROM UserInfoForConvert --Convert to SMALLINT 
SELECT CONVERT(VARCHAR(8),WriteDate,112) AS WriteDate FROM UserInfoForConvert --Convert to VARCHAR
SELECT CONVERT(int,EmpName) AS EmpName FROM UserInfoForConvert -- ERROR occurred.
```

### ***Converting to datetime data using CONVERT.***

- Dates and times have many formats to express, so they are very useful.
- Use style codes to convert between various expression formats by referring to example sources.

![Summary of MSSQL CONVERT function usage and performance degradation](/assets/images/postsImages/MsSql/1029_Eng_Fnc_CONVERTFunction/1.png)

- This is the running source.

```sql
--- Change date and time type
SELECT CONVERT(NVARCHAR, GETDATE(), 0)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 1)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 2)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 3)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 4)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 5)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 6)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 7)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 8)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 9)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 10)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 11)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 12)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 13)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 14)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 20)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 21)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 22)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 23)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 101)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 102)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 103)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 104)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 105)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 106)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 107)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 110)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 111)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 112)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 113)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 120)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 121)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 126)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 127)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 130)
UNION ALL SELECT CONVERT(NVARCHAR, GETDATE(), 131)
;
```

### ***How to use formatted XML in CONVERT.***

```sql
-- Using formatted XML with CONVERT
SELECT CONVERT(XML, '<root><child/></root>')
```

- The convert function is also available in xml format. 

### ***How to use INSERT, UPDATE, WHERE, LIKE.***

- The CONVERT function is mostly used within SELECT statements, but it is also used in a variety of other statements.

```sql
-- INSERT  Insert data of a specific type by converting it to another type
INSERT INTO UserInfoForConvert VALUES 
(111,'Jessica','20240101','10',CONVERT(NVARCHAR, GETDATE(), 110));

-- UPDATE   Used when changing the type of existing data
UPDATE UserInfoForConvert
   SET WriteDate = CONVERT(NVARCHAR, GETDATE(), 112)
 WHERE EmpID  =  '111';

-- WHERE, LIKE  
SELECT * 
  FROM DBO.UserInfoForConvert 
 WHERE EmpID  =  '111';
```

## <mark>Performance of the CONVERT function</mark>

### ***Factors that cause performance degradation of the CONVERT function***

- Performance degradation occurs when large data sets need to be converted.
- The index and optimization status of queries using the CONVERT function also affect performance, which can cause a load on the database.
- Frequent calls can cause a load, so queries that frequently call the CONVERT function should be used with caution.
