---
title: "How to use MSSQL Window Function"
excerpt: ""

categories:
  - MsSql
tags:
  - [clause]

## permalink: /MsSql///

toc: true
toc_sticky: true
 
date: 2024-07-27
last_modified_at: 2024-07-27
---

Window Functions allow you to easily define row-to-row relationships in database queries, making it easier to perform operations between rows.  
Other names include analysis function and ranking function.

## <mark>Window Function Structure</mark>

### ***Window Function Syntax***

```sql
SELECT WINDOW_FUNCTION (ARGUMENTS) OVER 
( [PARTITION BY column1,column2...] [ORDER BY column1,column2...] [WINDOWING clause] )
FROM table Name;
```

- `WINDOW_FUNCTION:` Contains window functions, aggregate functions, ranking functions, etc.
- `ARGUMENTS:` Depending on the function used, ARGUMENTS must be checked and entered.
- `PARTITION BY:` Divides the entire set of query results into partitions.
- `ORDER BY:` Use the order by clause when ranking column items.
- `WINDOWING:` The WINDOWING clause can strongly specify the standard range between lines that is the target of the function.

## <mark>How to use Window Function</mark>

### ***Rank function example.***

- `RANK:` After displaying the same rank for duplicate values, the value next to the duplicate rank is output in a rank separated by the number of duplicates.
- `DENSE_RANK:` Similar to the RANK function, but expresses the same rank as a single rank. In other words, even if there are duplicate ranks, the next rank value is output sequentially.
- `ROW_NUMBER:` Outputs unique ranks for the same value. Even if there is duplicate data, ranking can be handled.

```sql
--  RANK, DENSE_RANK, ROW_NUMBER    
SELECT classNo,studentNm,korScore
      ,RANK() OVER (PARTITION BY classNo ORDER BY classNo asc, korScore DESC ) AS winFuncRank
      ,DENSE_RANK() OVER (PARTITION BY classNo ORDER BY classNo asc, korScore DESC ) AS winFuncDenseRank
      ,ROW_NUMBER() OVER (PARTITION BY classNo ORDER BY classNo asc, korScore DESC ) AS winFuncRowNum
 FROM UserInfoForWindow
 WHERE scoreyyyymm = '202401';
```

![How to use Window Function](/assets/images/postsImages/MsSql/1020_Eng_clause_WINDOW/1.png)

### ***Aggregate function example in Window Function.***

- `SUM:` Outputs the total for each partition.
- `AVG:` Outputs the average for each partition.
- `COUNT:` Outputs the number of rows by partition.
- `MAX:` Prints the maximum value for each partition.
- `MIN:` Prints the minimum value for each partition.

```sql
-- SUM,AVG,COUNT,MAX, MIN
 SELECT classNo,studentNm,korScore
        ,SUM(korScore) OVER (partition by classNo  ORDER BY classNo asc) AS winFuncSum
        ,AVG(korScore) OVER (partition by classNo  ORDER BY classNo asc) AS winFuncAvg
        ,COUNT(korScore) OVER (partition by classNo  ORDER BY classNo asc) AS winFuncCnt
        ,MAX(korScore) OVER (partition by classNo  ORDER BY classNo asc) AS winFuncMax
        ,MIN(korScore) OVER (partition by classNo  ORDER BY classNo asc) AS winFuncMin
  FROM UserInfoForWindow
 WHERE scoreyyyymm = '202401';
```

![Aggregate function example in Window Function.](/assets/images/postsImages/MsSql/1020_Eng_clause_WINDOW/2.png)

### ***How to use WINDOWING in Window Function.***

- `ROWS:` Specifies a set of rows as a physical unit.
- `RANGE:` Specifies a row set by logical address.
- `BETWEEN ~ AND:` Used to specify the start and end positions of the partition row.
- `UNBOUNDED PRECEDING:` Specifies to start from the first row of the partition.
- `UNBOUNDED FOLLOWING:` Specifies that the partition ends on the last row.
- `CURRENT ROW:` Since it sets the row position at the execution point of the partition, CURRENT ROW can be specified at either the start or end.

```sql
-- UNBOUNDED PRECEDING: This means that the location for each partition is the first row.
-- UNBOUNDED FOLLOWING: This means that the location of each partition is the last row.
-- CURRENT ROW: This means that the starting position for each partition is the current row.
SELECT classNo,studentNm,korScore
      ,SUM(korScore) OVER (PARTITION BY classNo ORDER BY classNo,studentNm, korScore ASC
                           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS winUrcr
      ,SUM(korScore) OVER (PARTITION BY classNo ORDER BY classNo,studentNm, korScore ASC
                           ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS winFuncCrUf
      ,SUM(korScore) OVER (PARTITION BY classNo ORDER BY classNo 
                           ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS winFuncUpUf
 FROM UserInfoForWindow
 WHERE scoreyyyymm = '202401'
 ORDER BY classNo,studentNm,korScore;
```

![How to use WINDOWING in Window Function.](/assets/images/postsImages/MsSql/1020_Eng_clause_WINDOW/3.png)

## <mark>Advantages of Window Function</mark>

- By modularizing repeatedly used code, you can make the program concise and easy to understand.
- This is useful when obtaining rankings by partition or calculating aggregates within a specific range.
