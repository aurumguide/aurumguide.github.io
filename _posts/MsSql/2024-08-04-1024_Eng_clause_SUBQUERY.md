---
title: "How to use MSSQL SubQuery and its features"
excerpt: ""

categories:
  - MsSql
tags:
  - [clause]

## permalink: /MsSql///dd

toc: true
toc_sticky: true
 
date: 2024-08-04
last_modified_at: 2024-08-04
---

 

A SubQuery statement is another SQL statement contained within a main SQL statement. 
To help you understand, think of it as writing a query statement within a query statement.

## <mark>SubQuery Features</mark>

- SubQuery is divided into Un-Correlated and Correlated SubQuery depending on how it works.
- SubQuery is classified into single row SubQuery, multi-row SubQuery, and multi-column SubQuery depending on the return value.
- It has the advantage of being able to easily extract data with a single nested SQL statement, rather than having to perform repetitive queries to obtain results.
- It can be used in SELECT, FROM, WHERE, HAVING, and JOIN clauses.
- It can be used with most comparison operators, such as SQL operators =, <, >, IN, NOT IN, EXIST, NOT EXIST, etc.
- It can also be used in SELECT statements, including JOIN, VALUE in INSERT statements, SET in UPDATE statements, aggregate functions, and GROUP BY, and can also be used by nesting SubQuery.

## <mark>How to use SubQuery</mark>

### ***SubQuery is classified into two types depending on how it works.***

- An Un-Correlated SubQuery is a type of SubQuery where the SubQuery does not have any columns from the MainQuery.

```sql
--  Un-Correlated SubQuery
SELECT main.EmpID,main.EmpName,main.Title, main.DeptID
 FROM UserInfoForSubQuery main
 WHERE DeptID = (
              SELECT DeptID  
              FROM DeptInfoForSubQuery  sub
             where sub.DeptName = 'branch'
);
```

- A Correlated SubQuery is a type of SubQuery where the SubQuery contains columns from the MainQuery.
- In general, MainQuery is executed first and the read data is used to find data by entering conditions in SubQuery.

```sql
-- Correlated SubQuery
SELECT main.EmpID,main.EmpName,main.Title, main.DeptID
 FROM UserInfoForSubQuery main
 WHERE DeptID  =  (
    SELECT DeptID  
     FROM DeptInfoForSubQuery  sub
     where sub.DeptName = 'branch'
       and sub.DeptID  = main.DeptID  
);
```

![SubQuery is classified into two types depending on how it works.](/assets/images/postsImages/MsSql/1024_Eng_clause_SUBQUERY/1.png)

### ***Classified by the type of data returned.***

**Single Row SubQuery.**

- The execution result of the SubQuery is always 1 or less.
- If the number of results returned is 2 or more, the SQL statement will generate an execution time error.
- This SubQuery is used with single-row comparison operators (=, <, >, <=, >=, <>).

```sql
-- example
SELECT main.EmpID,main.EmpName,main.Title, main.DeptID
  FROM UserInfoForSubQuery main
 WHERE DeptID >= (
    SELECT DeptID  
      FROM DeptInfoForSubQuery  sub
  --  where sub.DeptName = 'branch'
);

-- example
SELECT main.EmpID,main.EmpName,main.Title, main.DeptID
  FROM UserInfoForSubQuery main
 WHERE DeptID >= (
    SELECT DeptID  
     FROM DeptInfoForSubQuery  sub
    where sub.DeptName = 'branch'
);
```

![Single Row SubQuery](/assets/images/postsImages/MsSql/1024_Eng_clause_SUBQUERY/2.png)

**Multi Row SubQuery.**

- A query statement that retrieves multiple rows. - SubQuery is executed independently from the main query and is used with multiple row comparison operators (IN, ALL, ANY, SOME, EXISTS).

```sql
 
SELECT main.EmpID,main.EmpName,main.Title, main.DeptID
  FROM UserInfoForSubQuery main
 WHERE exists (
  SELECT DeptID  
    FROM DeptInfoForSubQuery  sub
   where  sub.DeptID  = main.DeptID  
   and sub.DeptName = 'branch'
);

 
SELECT main.EmpID,main.EmpName,main.Title, main.DeptID
  FROM UserInfoForSubQuery main
 WHERE DeptID >= ANY (
   SELECT DeptID  
    FROM DeptInfoForSubQuery  sub
    --  where sub.DeptName = 'branch'
);
```

![Multi Row SubQuery](/assets/images/postsImages/MsSql/1024_Eng_clause_SUBQUERY/3.png)

### ***Classification by SubQuery location.***

**SubQuery in SELECT clause (scalar SubQuery).**

- When using a subquery in a SELECT clause, the output data of the SubQuery must be only one. 
- A scalar SubQuery is a SubQuery that returns only one row and one column.

```sql
 
SELECT main.EmpID
      ,main.EmpName
      ,main.Title
      ,main.DeptID
      ,( SELECT sub.DeptName 
          FROM DeptInfoForSubQuery sub 
          WHERE sub.DeptID = main.DeptID 
      ) as DeptName
 FROM UserInfoForSubQuery main;
```

**SubQuery in FROM (inline view).**

- The FROM clause usually requires a table name, but using SubQuery, you can use the retrieved values ​​as if they were a table.

```sql
SELECT main.EmpID
      ,main.EmpName
      ,main.Title
      ,main.DeptID
      ,sub.DeptName  DeptName
  FROM UserInfoForSubQuery main
   left outer join (
             SELECT DeptID,DeptName 
              FROM DeptInfoForSubQuery s 
     ) sub
   on main.DeptID =  sub.DeptID
```

**SubQuery in WHERE.**

- You can use subqueries in the WHERE clause to filter values ​​that satisfy a condition.

```sql
 
SELECT main.EmpID,main.EmpName,main.Title, main.DeptID
  FROM UserInfoForSubQuery main
 WHERE DeptID = some(
      SELECT DeptID  
       FROM DeptInfoForSubQuery  sub 
);
```

## <mark>SubQuery Constraints</mark>

1. SubQuery must be used in parentheses, and the Order By clause cannot be used within a subquery.
2. SubQuery must be used on the right side of an operator.
