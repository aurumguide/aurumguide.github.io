---
title: "How to use MSSQL GROUP BY clause"
excerpt: ""

categories:
  - MsSql
tags:
  - [clause]

## permalink: /MsSql///

toc: true
toc_sticky: true
 
date: 2024-07-25
last_modified_at: 2024-07-25
---

The GROUP BY clause is used to group specific columns when extracting data from a database and to output data using an aggregate function within the group.  
The most commonly used aggregate functions are SUM, COUNT, AVG, MAX, MIN, STDEV, and STRING_GAG.

## <mark>Basic structure of the GROUP BY clause</mark>

COLUMN, excluding aggregate functions, must be declared in both the SELECT clause and the GROUP BY clause.

1. `SELECT:` Select the COLUMN to retrieve data. It is also possible to use multiple aggregate functions at the same time.
2. `FROM:` Declare the table from which to import data.
3. `GROUP BY:` Declare the COLUMN to be grouped.
4. `ORDER BY:` Specify when ordering is different from the default GROUP BY order.


![structure of the GROUP BY clause](/assets/images/postsImages/MsSql/1019_Eng_clause_GROUPBY/1.png)

## <mark>Example of using GROUP BY clause</mark>

### ***Basic GROUP BY clause.***

- This is an example of printing the average Korean language score for each class using the AVG function.

```sql
-- Example of finding a student's average score in Korean in January 2024.
SELECT classNo
      ,AVG(korScore) AS korScoreAvg
 FROM UserInfoForGroupBy
WHERE scoreyyyymm = '202401'
GROUP BY classNo
order by classNo asc;
```

![Basic GROUP BY clause.](/assets/images/postsImages/MsSql/1019_Eng_clause_GROUPBY/2.png)

### ***Using GROUP BY with multiple group COLUMNs.***

- This is an example of grouping students' grades by month and class and outputting the average Korean score using the AVG function.
- Specifying multiple groupings increases the number of data rows.

```sql
-- Example of calculating the average Korean language score for each month and class.
SELECT scoreyyyymm
      ,classNo
      ,AVG(korScore) AS korScoreAvg
 FROM UserInfoForGroupBy
WHERE scoreyyyymm  IN ('202401','202402')
GROUP BY classNo ,scoreyyyymm
order by scoreyyyymm ASC, classNo DESC;
```

![Using GROUP BY with multiple group COLUMNs.](/assets/images/postsImages/MsSql/1019_Eng_clause_GROUPBY/3.png)

### ***How to use multiple aggregate functions.***

This is an example of grouping students' grades by month and class and then calculating the overall average score using the COUNT, SUM, and AVG functions.

```sql
-- Example of printing subject totals and overall average scores by month and class.
SELECT scoreyyyymm
      ,classNo
      ,COUNT(*) AS groupCount
      ,sum(korScore) AS korScoreSum
      ,sum(mathScore) AS mathScoreSum
      ,sum(engScore) AS engScoreSum
      ,sum(scienceScore) AS scienceScoreSum
      ,sum(societyScore) AS societyScoreSum
      ,AVG(korScore + mathScore + engScore + scienceScore + societyScore) / 5 AS ScoreAvg
 FROM UserInfoForGroupBy
WHERE scoreyyyymm  IN ('202401','202402')
GROUP BY classNo ,scoreyyyymm
order by scoreyyyymm ASC, classNo DESC;
```

![How to use multiple aggregate functions.](/assets/images/postsImages/MsSql/1019_Eng_clause_GROUPBY/4.png)

### ***How to use GROUP BY and ORDER BY at the same time.***

You can use an ORDER BY clause after a GROUP BY clause, and use group columns and aggregate functions to specify sorting in the ORDER BY clause.

```sql
-- Please use ORDER BY to display from the most recent month.
SELECT scoreyyyymm
      ,classNo
      ,sum(korScore) AS korScoreSum
 FROM UserInfoForGroupBy
WHERE scoreyyyymm  IN ('202401','202402')
GROUP BY classNo ,scoreyyyymm
order by scoreyyyymm DESC, classNo asc;
```

![How to use GROUP BY and ORDER BY at the same time.](/assets/images/postsImages/MsSql/1019_Eng_clause_GROUPBY/5.png)

### ***How to use HAVING in the GROUP BY clause.***

If you want to use the result of an aggregate function in a conditional clause without using a sub-query statement, use the HAVING clause.
Using the HAVING clause and AVG, scores of 80 or higher can only be output for the class.

```sql
-- Using HAVING, only Korean average scores of 80 or higher are output.
SELECT scoreyyyymm
      ,classNo
      ,avg(korScore) AS korScoreSum
 FROM UserInfoForGroupBy
WHERE scoreyyyymm  IN ('202401','202402')
GROUP BY classNo ,scoreyyyymm 
HAVING  avg(korScore) > 80;
```

![How to use HAVING in the GROUP BY clause.](/assets/images/postsImages/MsSql/1019_Eng_clause_GROUPBY/6.png)

### ***How to combine COLUMN strings using STRING_AGG in the GROUP BY clause.***

STRING_AGG is a function that allows you to combine row data by putting separator values ??in the form of a string into one column.

```sql
-- How to combine COLUMN strings using STRING_AGG
SELECT classNo
      ,STRING_AGG(studentNm,',') AS studentNm
 FROM UserInfoForGroupBy
WHERE scoreyyyymm  IN ('202401')
  AND classNo  IN (1,2)
GROUP BY classNo;
```

![How to combine COLUMN strings using STRING_AGG in the GROUP BY clause.](/assets/images/postsImages/MsSql/1019_Eng_clause_GROUPBY/7.png)

## <mark>Aggregation function description</mark>

### ***Basic aggregate function supported by SQL SERVER.***

| **aggregate function name** | **COMMNET** |
| --- | --- |
| COUNT | Display COUNT in specified group. |
| SUM | Display totals in specified groups. |
| AVG | Displays the average value in a specified group. |
| MAX | Displays the maximum value in a specified group. |
| MIN | Displays the minimum value in the specified group. |
| STDEV | Used to find the standard deviation in a specified group. |
| STRING_AGG | Combine ROW DATA into a string in a specified group. |

