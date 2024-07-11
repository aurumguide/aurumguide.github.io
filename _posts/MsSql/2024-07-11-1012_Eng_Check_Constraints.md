---
title: "Creation, verification, delete of MSSQL check constraints"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql,Constraints]

## permalink: /MsSql///

toc: true
toc_sticky: true
 
date: 2024-07-11
last_modified_at: 2024-07-11
---

The CHECK constraint is a constraint that can be rejected if the input data meets the CHECK condition, and if true for the input data, if false.

## <mark>Characteristics of Check Constraints</mark>

- The MSSQL CHECK constraint is used to specify a range of inputtable values for a particular column.
- If you set a CHECK constraint on one column, that column can only enter values within a specific range.
- Then, if you set the CHECK constraint in one table, you can also restrict the value of another column based on the specific column of that record.

## <mark>Add CHECK Constraints</mark>

### ***Automatically set check constraints on column while creating TABLE.***

- While creating a table, you can add constraints after the check schedule after column, data type.
- The check constraint is not often used, but is available when you specify a range of columns.

```sql
-- syntax
CREATE TABLE [table name] (
      [column1] [dataType],
      [column2] [dataType] CHECK ([conditional clause])
   );


-- 1. Automatically set up CHECK in column while creating TABLE.
USE sampleDB;
DROP TABLE IF EXISTS AurumGuideForCheck;
CREATE TABLE AurumGuideForCheck (
    UserId int,
    UserName varchar(255),
    UserAge  int CHECK (UserAge > 20 and UserAge < 65)    
);

-- CHECK Check if the constraints are correct.
insert into AurumGuideForCheck(UserId,UserName,UserAge)
values(202401,'john',10);
```

- How to generate check constraints while generating table Source code and example.

![CHECK Check if the constraints are correct.](/assets/images/postsImages/MsSql/1012_Eng_CHECK_Constraints/check_constraints.jpg)

### ***To create a CHECK constraint using more than one column.***

- When creating a table, declare a column first, then create a check constraint on multiple columns by construct.
- Please enter the check constraint name and add the condition using the column name.
- After checking the syntax and source code of the check constraints, practice the source code.

```sql
-- syntax.
CREATE TABLE [table name] (
    [column1] [dataType],
    [column2] [dataType],
    [column3] [dataType],
CONSTRAINT [check constraint name] CHECK([conditional clause]) 
);

-- 2. Create CHECK constraints using more than one column.
USE sampleDB;
DROP TABLE IF EXISTS AurumGuideForCheck;
CREATE TABLE AurumGuideForCheck (
    UserId int,
    UserName varchar(255),
    UserFirstName  varchar(255),
    UserAge  int,
    CONSTRAINT Ck_AurumGuideForCheck CHECK(UserAge > 10 and UserFirstName = 'john')  
);
```

### ***Create a CHECK constraint on an existing column.***

- You can create check constraints even after you create a table.
- Try adding a check constraint to an existing table using the alternator command and the check reservation.
- Check the example source code before you practice.

```sql
-- syntax.
ALTER TABLE [table name] ADD CONSTRAINT [check constraint name] CHECK ([conditional clause]);

-- 3. Create CHECK Constraints on Existing Columns.
USE sampleDB;
DROP TABLE IF EXISTS AurumGuideForCheck;
CREATE TABLE AurumGuideForCheck (
    UserId int,
    UserName varchar(255),
    UserFirstName  varchar(255),
    UserAge  int 
);
go
ALTER TABLE AurumGuideForCheck  ADD CONSTRAINT Ck_AurumGuideForCheck CHECK(UserAge > 10 and UserFirstName = 'john') ;
```

### ***Added CHECK constraints when adding new columns.***

- This is a method of adding check constraints while adding columns to existing tables.
- Add a column using the altern command and add the check condition.
- Check the example source code before you practice.

```sql
-- syntax.
ALTER TABLE [table name] ADD [column1] [dataType] CONSTRAINT [check constraint name] CHECK ([conditional clause]);

-- 4. Example of check settings when adding a new column.
USE sampleDB;
DROP TABLE IF EXISTS AurumGuideForCheck;
CREATE TABLE AurumGuideForCheck (
    UserId int,
    UserName varchar(255) 
);
go

ALTER TABLE AurumGuideForCheck ADD UserAge int  CONSTRAINT Ck_AurumGuideForCheck CHECK(UserAge > 10);
```

## <mark>How to check created CHECK constraints</mark>

- EXEC SP\_HELP \[table name\].
- system view,sys.check\_constraints
- Practice check constraints with two conditions.

```sql
-- sp_help.
EXEC sp_help  AurumGuideForCheck;
GO
-- check_constraints.
select schema_name(t.schema_id) + '.' + t.[name] as table_nm
    ,col.[name] as column_nm
    ,con.[name] as constraint_nm 
    ,con.[definition] as df_value
from sys.check_constraints  con
left outer join sys.objects t
on con.parent_object_id = t.object_id
left outer join sys.all_columns col
on con.parent_column_id = col.column_id
and con.parent_object_id = col.object_id 
order by con.name;
```

## <mark>Delete the created CHECK constraint</mark>

- Once you have created a check constraint, you can also drop it.
- Deleting a check constraint is possible after using the check constraint section because the name of the check constraint is required.
- Please practice using the check constraint sytax and source code.

```sql
ALTER TABLE [table name] DROP CONSTRAINT [check constraint name];

-- Delete the created CHECK constraint
USE sampleDB;
DROP TABLE IF EXISTS AurumGuideForCheck;
CREATE TABLE AurumGuideForCheck (
    UserId int,
    UserName varchar(255),
    UserFirstName  varchar(255),
    UserAge  int,
    CONSTRAINT Ck_AurumGuideForCheck CHECK(UserAge > 10 and UserFirstName = 'john')  
);

ALTER TABLE AurumGuideForCheck  DROP CONSTRAINT Ck_AurumGuideForCheck;
```

## <mark>Check Constraint Finalization</mark>

- Check constraints are not often used in practice.
- If a problem occurs because incorrect data is periodically entered during database operation, the problem can be solved by adding a check constraint.
