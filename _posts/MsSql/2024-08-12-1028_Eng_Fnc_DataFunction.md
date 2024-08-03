---
title: "How to use and explain MSSQL date and time functions"
excerpt: ""

categories:
  - MsSql
tags:
  - [MSSQL FUNCTION]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-08-12
last_modified_at: 2024-08-12
---

MSSQL supports various date and time functions, so I will explain how to use them.

## <mark>Date function, time function features</mark>

- It is useful for extracting, calculating, and converting dates and times from a database.
- You should be able to convert date and time functions into the desired format.

## <mark>Date function, time function usage and explanation</mark>

### ***ISDATE() function.***

- Used when checking if it can be converted to a date function. 
- If the parameter value is in date format, it returns '1', but if it is not a date, it returns '0'.

```SQL
 -- ISDATE()  function.
SELECT ISDATE(GETDATE()) AS dateCheckIsOk
      ,ISDATE('NoDate')  AS dateCheckIsNo
;
```

### ***GETDATE() function.***

- This function returns the current system date and time set in the database.
- This is a non-deterministic function, so it returns the current value based on the system time each time it is called.
- Although it is not used often, check out the GETUTCDATE() , SYSDATETIME() , SYSDATETIMEOFFSET() , SYSUTCDATETIME() , and CURRENT_TIMESTAMP functions.

```SQL
 -- GETDATE()  function.
 SELECT GETDATE()
       ,GETUTCDATE()
       ,SYSDATETIME()
       ,SYSDATETIMEOFFSET()
       ,SYSUTCDATETIME()
       ,CURRENT_TIMESTAMP
;
```

### ***YEAR(), MONTH(), DAY() function.***

- The YEAR() function returns the year from the date you enter.
- The MONTH() function returns the month from the date you enter.
- The DAY() function returns the day from the date you enter.

```SQL
-- YEAR(),MONTH(),DAY() function
SELECT GETDATE()        AS 'FULL_DATE'
      ,YEAR(GETDATE())  AS 'YEAR'
      ,MONTH(GETDATE()) AS 'MONTH'
      ,DAY(GETDATE())   AS 'DAY'
;
```

### ***DATEPART(), DATENAME() function.***

- The DATEPART() and DATENAME() functions are used to extract the year, month, day, hour, minute, second, day of the week, quarter, day of the year, and week of the year from date data.

```SQL
-- 2024-04-13 23:39:33.260  DATEPART(), DATENAME() function
SELECT DATEPART(YEAR, GETDATE())      AS 'DATEPART_YEAR'
      ,DATENAME(YEAR, GETDATE())      AS 'DATENAME_YEAR'
      ,DATEPART(MONTH, GETDATE())     AS 'DATEPART_MONTH'
      ,DATENAME(MONTH, GETDATE())     AS 'DATENAME_MONTH'
      ,DATEPART(DAY, GETDATE())       AS 'DATEPART_DAY'
      ,DATENAME(DAY, GETDATE())       AS 'DATENAME_DAY'
      ,DATEPART(HOUR, GETDATE())      AS 'DATEPART_HOUR'
      ,DATENAME(HOUR, GETDATE())      AS 'DATENAME_HOUR'
      ,DATEPART(MINUTE, GETDATE())    AS 'DATEPART_MINUTE'
      ,DATENAME(MINUTE, GETDATE())    AS 'DATENAME_MINUTE'
      ,DATEPART(SECOND, GETDATE())    AS 'DATEPART_SECOND'
      ,DATENAME(SECOND, GETDATE())    AS 'DATENAME_SECOND'
      ,DATEPART(WEEKDAY, GETDATE())   AS 'DATEPART_WEEKDAY'
      ,DATENAME(WEEKDAY, GETDATE())   AS 'DATENAME_WEEKDAY'
      ,DATEPART(QUARTER, GETDATE())   AS 'DATEPART_QUARTER'
      ,DATENAME(QUARTER, GETDATE())   AS 'DATENAME_QUARTER'
      ,DATEPART(DAYOFYEAR, GETDATE()) AS 'DATEPART_DAYOFYEAR'
      ,DATENAME(DAYOFYEAR, GETDATE()) AS 'DATENAME_DAYOFYEAR'
      ,DATEPART(WEEK, GETDATE())      AS 'DATEPART_WEEK'
      ,DATENAME(WEEK, GETDATE())      AS 'DATENAME_WEEK'
;
```

![DATEPART(), DATENAME() function](/assets/images/postsImages/MsSql/1028_Eng_Fnc_DataFunction/1.png)

### ***DATEADD() function.***

- This is a function used when performing operations on dates or times.
- It is mainly used to add or subtract a specified interval to a date.
- The function format is DATEADD(datepart, number, inputDate).
- Datepart specifies a time interval. For example, 'year' represents a year.
- Number is the value to be added or subtracted.
- InputDate is the reference date.

```SQL
/*
DATEADD() function
YEAR (Year or yy)
Monthly월(Month or mm)
daily unit (Day or dd)
Weekly(Week or ww)
HOUR, Minutes Seconds (Hour, Minute, Second)
*/
SELECT DATEADD(YEAR, 2, GETDATE()) AS '2 years from GETDATE'
      ,DATEADD(yy, -2, GETDATE()) AS '2 years from GETDATE'
      ,DATEADD(MONTH, 3, GETDATE()) AS '3 months from GETDATE'
      ,DATEADD(mm, -5, GETDATE()) AS '5 months from GETDATE'
      ,DATEADD(DAY, 5, GETDATE()) AS '5 days from GETDATE'
      ,DATEADD(dd, -8, GETDATE()) AS '8 days from GETDATE'
      ,DATEADD(WEEK, 1, GETDATE()) AS '1 week from GETDATE'
      ,DATEADD(ww, -2, GETDATE()) AS '2 weeks ago from GETDATE'
      ,DATEADD(HOUR, 8, GETDATE()) AS '8 hours ago from GETDATE'
      ,DATEADD(MINUTE, -70, GETDATE()) AS '70 minutes ago from GETDATE'
      ,DATEADD(SECOND, 20, GETDATE()) AS '20 seconds ago from GETDATE'
      ;
```

### ***DATEDIFF() Fnuction.***

- The function format is DATEDIFF(datepart, startDate, endDate).
- Compares dates and calculates the difference based on the DATEPART delimiter, returning the result as an integer.
- datepart specifies a time interval. For example, 'month' represents a month.
- startDate is the start date.
- endDate is the end date.

```sql
/*
-- DATEDIFF() Fnuction
DAY:  Returns the number of days.
MONTH: Returns the number of times the month has changed.
YEAR: Returns the number of times the year has changed.
*/
SELECT DATEDIFF(DAY   ,DATEADD(YEAR, -2, GETDATE()), GETDATE())   AS 'Returns the number of days'
      ,DATEDIFF(MONTH ,DATEADD(YEAR, -2, GETDATE()), GETDATE())   AS 'Returns the number of times the month has changed'
      ,DATEDIFF(YEAR  ,DATEADD(YEAR, -2, GETDATE()), GETDATE())   AS 'Returns the number of times the year has changed'
;
```

## <mark>Organizing date and time functions.</mark>

### ***How to display DATEPART.***

| **Type** | **DATEPART(full)** | **DATEPART(short)** |
| --- | --- | --- |
| **YEAR Unit**| YEAR | YYYY,YY |
| **Monthly** | MONTH | MM, M |
| **daily Unit** | DAY | DD, D |
| **Hour Unit** | HOUR | HH |
| **Minutes** | MINUTE | MI, N |
| **Seconds** | SECOND | SS, S |
| **MILLI** | MILLISECOND | MS |
| **Weekly Unit** | WEEK | WK |
| **Quarterly** | QUARTER | QQ, Q |
