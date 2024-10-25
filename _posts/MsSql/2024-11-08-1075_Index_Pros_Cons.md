---
title: "MSSQL Index Pros and Cons"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Indexe]

## permalink: /MsSql////  ////

toc: true
toc_sticky: true
 
date: 2024-11-08
last_modified_at: 2024-11-08
---

Indexes can improve the performance of a database, but they can also cause performance degradation. So today, I will explain the pros and cons of indexes.

## <mark>Advantages of INDEX</mark>

- You can improve the speed of searching and sorting in a table based on the primary key.
- Using an index can strengthen the uniqueness of table rows.
- The primary key of a table is automatically indexed.
- Some fields cannot be indexed due to their data type. A representative field is text type.
- Using a multi-field index is a criterion that can distinguish records with the same first field value, and up to 10 are possible.

## <mark>Disadvantages of INDEX</mark>

- Indexes also require storage space, so they take up disk storage space.
- Performance degrades when updating data in INDEX fields, or adding or deleting records.
- INDEX requires periodic management, which means it incurs costs.
- It is not easy to change the index when there is a large amount of data. You must be careful when creating it for the first time.
- When data changes occur frequently, the index needs to be rebuilt, which can affect performance.
- Adding an INDEX will speed up the query speed, but the speed of adding data rows will be slow, which may cause record locking problems when used by multiple users.
- Tables that frequently INSERT, DELETE, and UPDATE require performance testing in advance.

## <mark>When to use an INDEX</mark>

- In order to use an INDEX efficiently, it is better to use it for columns that have a wide data range and less duplication, or columns that are frequently searched or useful for being sorted.
- It is good to create an index for large tables. - It is a good idea to create an index on columns that do not frequently have INSERT, UPDATE, or DELETE operations.
- It is a good idea to create an index on columns that frequently have WHERE, ORDER BY, JOIN, etc.
- It is a good idea to create an index on columns that have low data duplication.

![When is it good to use INDEX?](/assets/images/postsImages/MsSql/1075_Index_Pros_Cons/1.png)
