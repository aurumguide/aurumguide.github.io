---
title: "How to use MSSQL CAST function"
excerpt: ""

categories:
  - MsSql
tags:
  - [MSSQL FUNCTION]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-08-16
last_modified_at: 2024-08-16
---


The CAST function supported in MSSQL is a function used when converting data types, similar to the CONVERT function.

## <mark>Features of the CAST function</mark>

- The CAST function is used to convert data from one type to another.
- It is useful when converting a string to a number or a date format to another format.
- The CAST function is mainly used to simply convert data types.

## <mark>How to use and explain the CAST function</mark>

### ***Syntax of the CAST function.***

```sql
CAST( expression AS data_type [(length)])
```

- data_type: The data type to which you want to convert the data.
- length: The length of the returned data type.
- expression: The actual data or field to convert.

### ***How to change data type.***

- This is a method to convert a string to a number using the CAST function.

```sql
-- UserInfoForCast
DROP TABLE IF EXISTS UserInfoForCast;
CREATE TABLE dbo.UserInfoForCast
(
    EmpID SMALLINT, 
    EmpName NVARCHAR(40), 
    InputDate VARCHAR(8),
    DeptID INT,
    WriteDate datetime
); 

INSERT INTO UserInfoForCast VALUES 
(110,'Jessica','20240101','10',getdate()),
(111,'Alicia','20240301','10',getdate()),
(120,'Michael','20240401','20',getdate()),
(121,'Alicia','20240914','20',getdate()) ;

 
SELECT CAST(EmpID AS INT) AS EmpID
  FROM UserInfoForCast;
```

- Here's how to convert a date to a string using the CAST function.

```sql
SELECT CAST(WriteDate AS VARCHAR) AS EmpID
 FROM UserInfoForCast;
```

- How to use arithmetic operators in CAST.

```sql
SELECT CAST(ROUND(EmpID / DeptID, 0) AS INT) AS clacColumn
 FROM UserInfoForCast;
```

- How to concatenate strings using CAST.

```sql
SELECT 'The CAST name is ' + CAST(EmpName AS VARCHAR(12)) AS ListPrice
 FROM dbo.UserInfoForCast
```

- Using formatted XML in CAST.

```sql
SELECT CAST('<root><child/></root>' AS XML);
```

![How to use MSSQL CAST function](/assets/images/postsImages/MsSql/1030_Eng_Fnc_CASTFunction/1.png)

## <mark>Notes when converting between data types</mark>

### ***Check if the data is convertible.***

- When converting a string to a number, you must check the data to see if it consists of only numbers before using it. - If there are characters, an error occurs.

### ***Check when converting date and time.***

- When converting date and time, check the format exactly and convert it.

### ***Check for data loss when converting strings.***

- When converting strings, check the length and cut it off or check the maximum length.
- When converting strings to other data types, you should consider encoding and character sets to prevent data loss.

### ***Errors due to NULL data.***

- You should decide how to handle null values ​​in data.
- If you do not set a processing method, errors often occur during conversion.

### ***Factors that cause performance degradation of the CAST function.***

- Performance degradation occurs when a large data set must be converted.
- The index and optimization status of the query using the CAST function also affect performance, which can cause a load on the database.
- Frequent calls can cause a load, so queries that frequently call the CAST function should be used with caution.
