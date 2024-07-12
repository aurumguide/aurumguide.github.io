---
title: "How to create, check, and delete MSSQL NOT NULL constraints"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql,Constraints]

## permalink: /MsSql//a

toc: true
toc_sticky: true
 
date: 2024-07-13
last_modified_at: 2024-07-13
---

NOT NULL constraints require data to be entered when entering or modifying.

## <mark>NOT NULL constraint features</mark>

- An ERROR occurs when NULL is entered in a column to which the NOT NULL constraint is applied. 
- In other words, a value always exists in the column.
- Data Integrity: NOT NULL constraints help maintain data integrity. 
- For example, you can maintain integrity by applying a NOT NULL constraint to columns that store required input information such as user ID, name, and address.
- When inserting data into a column with a NOT NULL constraint, an error will occur if a value is not specified for the column. 
- To prevent this, NOT NULL is set as the default and is mainly used.
- ALTER TABLE can be changed using the ALTER TABLE statement to add or remove a NOT NULL constraint to a column in an already existing table.

## <mark>How to use NOT NULL constraint</mark>

### ***Automatically sets CHECK on column while creating TABLE.***

- When creating a table, you can use the NOT NULL constraint after the column and datatype.
- The NOT NULL constraint is a constraint frequently used when creating tables.
- Mainly used in columns related to keys.
- Please practice using syntax code and source code.

```sql
-- syntax code 
CREATE TABLE [table name] (
      [column1] [dataType],
      [column2] [dataType] NOT NULL,
      [column3] [dataType] NOT NULL
   );

-- 1. When creating a TABLE, automatically set NOT NULL to the column.
USE sampleDB;
DROP TABLE IF EXISTS AurumGuideForNotNull;
CREATE TABLE AurumGuideForNotNull (
    UserId int,
    UserNm varchar(255) NOT NULL,
    UserAge  int   NOT NULL, 
    UserAddress varchar(500) NOT NULL
);

-- Check if the NOT NULL constraint is working.
insert into AurumGuideForNotNull(UserId,UserNm,UserAge,UserAddress)
values(202401,'john',10,NULL);
```

![Set not null in Column.](/assets/images/postsImages/MsSql/1013_Eng_NOTNULL_Constraints/1.jpg)

### ***Create NOT NULL constraint on an existing column.***

- Similar to other constraints, you can specify not nul in a column through the alter command.
- The not nul condition is created after the datatype reserved word.

```sql
-- syntax.
ALTER TABLE [table name] ALTER COLUMN [column1] [dataType] NOT NULL;

USE sampleDB;
DROP TABLE IF EXISTS AurumGuideForNotNull;
CREATE TABLE AurumGuideForNotNull (
    UserId int,
    UserNm varchar(255),
    UserAge  int, 
    UserAddress varchar(500)
);
 
ALTER TABLE AurumGuideForNotNull ALTER COLUMN UserAddress VARCHAR(500) NOT NULL;
```

### ***When an existing column contains NULL values.***

- If the column you want to change is null, the property cannot be changed to NOT NULL.
- As a solution, you need to add a value to the existing column.
- This is an example of an error occurring.
- Try using images to generate errors through practice.

![An error occurs when changing the column property with a null value.](/assets/images/postsImages/MsSql/1013_Eng_NOTNULL_Constraints/2.jpg)

### ***How to resolve NULL value in column.***

- It would be nice to include a not null constraint when creating a table, but if the table contains null values, additional work is required.
- You must check the existing data and delete or modify the row.
- If modification is necessary, change the value of the corresponding column using the NOT NULL constraint.
- Understand and practice the example source first.

```sql
USE sampleDB;
DROP TABLE IF EXISTS AurumGuideForNotNull;
CREATE TABLE AurumGuideForNotNull (
    UserId int,
    UserNm varchar(255),
    UserAge  int , 
    UserAddress varchar(500)
);

insert into AurumGuideForNotNull(UserId,UserNm,UserAge,UserAddress)
values(202401,'john',10,NULL);
insert into AurumGuideForNotNull(UserId,UserNm,UserAge,UserAddress)
values(202402,'john1',10,NULL);

-- Check for null values in existing columns.
select *
 from AurumGuideForNotNull   
where UserAddress is null;

-- null value in existing column => Changed to ''.
update AurumGuideForNotNull
   set UserAddress  = ''
where UserAddress is null;
 
-- change to existing column.
ALTER TABLE AurumGuideForNotNull ALTER COLUMN UserAddress VARCHAR(500) NOT NULL;
```

- This is an explanation of the execution process.

![Explanation of the execution process.](/assets/images/postsImages/MsSql/1013_Eng_NOTNULL_Constraints/3.jpg)

### ***How to check NOT NULL constraint creation.***

- You can use the EXEC SP_HELP [table name] command..
- Use the INFORMATION_SCHEMA.COLUMNS schema.
- When finding the constraints of one table, use the sp_help command.
- Use INFORMATION_SCHEMA.COLUMNS to find which tables in the database use constraints.

```sql
-- instruction code
EXEC SP_HELP AurumGuideForNotNull;
 
-- INFORMATION_SCHEMA.COLUMNS   
SELECT  TABLE_CATALOG 
       ,TABLE_SCHEMA 
       ,TABLE_NAME 
       ,IS_NULLABLE
from INFORMATION_SCHEMA.COLUMNS
where TABLE_NAME  = 'AurumGuideForNotNull';
```

### ***Delete the created NOT NULL constraint.***

- If you have created a Not null constraint, you can also drop it.
- Please practice using the Not null constraint syntax and source code.

```sql
ALTER TABLE [table name] ALTER COLUMN [column1] [dataType];

USE sampleDB;
DROP TABLE IF EXISTS AurumGuideForNotNull;
CREATE TABLE AurumGuideForNotNull (
    UserId int,
    UserNm varchar(255) NOT NULL,
    UserAge  int   NOT NULL,
    UserAddress varchar(500) NOT NULL
);

-- remove not null
ALTER TABLE AurumGuideForNotNull ALTER COLUMN UserAddress VARCHAR(500);
```

## <mark>Not null constraint finalization</mark>

- Not null constraints are mainly used when creating tables.
- When adding a Not null constraint during operation, check the existing data and change the constraint.
