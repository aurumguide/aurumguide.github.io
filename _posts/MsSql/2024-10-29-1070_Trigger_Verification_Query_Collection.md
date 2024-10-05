---
title: "MSSQL Trigger Verification Query Collection"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Trigger]

## permalink: /MsSql///////

toc: true
toc_sticky: true
 
date: 2024-10-29
last_modified_at: 2024-10-29
---
 
This is a collection of queries that are absolutely necessary in the field related to MSSQL Triggers.

## <mark>If you want to study Triggers, please refer to the link below.</mark>

- [MSSQL Trigger Features and Types.](https://aurumguide.com/mssql/1058_Trigger_Features_and_Types/ "Trigger Features and Types.")
- [MSSQL LOGIN Trigger Usage and Features.](https://aurumguide.com/mssql/1059_LOGIN_Trigger_Usage_and_Features/ "LOGIN Trigger Usage and Features.")
- [MSSQL DDL Trigger Usage and Features.](https://aurumguide.com/mssql/1060_DDL_Trigger_Usage_Features/ "DDL Trigger Usage and Features.")

- [MSSQL DML Trigger Features and Types.](https://aurumguide.com/mssql/1061_DML_Trigger_Features_Types/ "DML Trigger Features and Types.")

- [How to create, modify, and delete MSSQL AFTER Trigger](https://aurumguide.com/mssql/1062_Trigger_DML_AFTER/ "How to create, modify, and delete MSSQL AFTER Trigger.").
- [How to create, modify, and delete MSSQL INSTEAD OF Trigger.](https://aurumguide.com/mssql/1063_Trigger_DML_INSTEADOF/ "How to create, modify, and delete MSSQL INSTEAD OF Trigger.")
- [MSSQL insert, update, delete simultaneous trigger usage.](https://aurumguide.com/mssql/1064_Trigger_INSERT_UPDATE_DELETE/ "insert, update, delete simultaneous trigger usage.")
- [How to Use MSSQL Nested Triggers.](https://aurumguide.com/mssql/1065_Nested_Triggers/ "How to Use MSSQL Nested Triggers.")
- [How to use MSSQL Trigger Function and its features.](https://aurumguide.com/mssql/1066_Trigger_Function/ "How to use MSSQL Trigger Function and its features.")
- [How to use and explain MSSQL Trigger enable, disable.](https://aurumguide.com/mssql/1067_Trigger_enable_disable/ "How to use and explain MSSQL Trigger enable, disable.")
- [MSSQL Trigger query and content search.](https://aurumguide.com/mssql/1068_Trigger_query_content_search/ "Trigger query and content search.")
- [MSSQL Trigger Pros and Cons.](https://aurumguide.com/mssql/1069_Trigger_Pros_Cons/ "Trigger Pros and Cons.") 

### ***You can check the TRIGGER status of server, DML, and DATABASE.***

- When the trigger is not documented, use it to check all triggers with one query.
- This is the source code.

```sql
-- You can check the TRIGGER status of server, DML, and DATABASE.
SELECT 'DML,Database' name_type,
       name,
       parent_class_desc,     
       case when is_disabled  = 0 then 'disable' else 'enable' end tr_status
FROM sys.triggers
UNION ALL
SELECT 'server tr' name_type,
       name,
       parent_class_desc,    
       case when is_disabled  = 0 then 'disable' else 'enable' end tr_status
FROM sys.server_triggers
-- WHERE is_disabled = 1
;
```

### ***Find out about TRIGGERs created in TABLE and VIEW.***

- Use this to check which table a trigger is created in.
- This is the source code.

```sql
-- Find out about TRIGGER created in TABLE and VIEW.
SELECT triggers.name [Trigger_name],
       tables.name [Parent_table_name],                 
       views.name [Parent_view_name]
FROM sys.triggers triggers
LEFT JOIN sys.tables tables
    ON triggers.parent_id = tables.object_id
LEFT JOIN sys.views views
    ON triggers.parent_id = views.object_id
WHERE triggers.parent_class = 1
;
```

### ***Check trigger information through dm_exec_trigger_stats.***

- dm_exec_trigger_stats is a system view that lets you know the status of a trigger.
- Use it when you want to know the statistics and status of a trigger when operating a DB.
- This is the source code.

```sql
-- Check trigger information through dm_exec_trigger_stats.
SELECT QUOTENAME(DB_NAME(dm_exec_trigger_stats.database_id)) + '.'+
       QUOTENAME(ISNULL(OBJECT_SCHEMA_NAME(dm_exec_trigger_stats.object_id, dm_exec_trigger_stats.database_id), 'Server Trigger')) + '.'+
       QUOTENAME(ISNULL(OBJECT_NAME(dm_exec_trigger_stats.object_id, dm_exec_trigger_stats.database_id),server_triggers.name)) trigger_info,
       OBJECT_SCHEMA_NAME(dm_exec_trigger_stats.object_id, dm_exec_trigger_stats.database_id) schemaName,
       OBJECT_NAME(dm_exec_trigger_stats.object_id, dm_exec_trigger_stats.database_id) objectName,

       --  Pages spilled by trigger.
       dm_exec_trigger_stats.total_spills as total_spills,
       dm_exec_trigger_stats.last_spills  as last_spills,
       dm_exec_trigger_stats.min_spills   as min_spills,
       dm_exec_trigger_stats.max_spills   as max_spills, 

       -- Getting execution statistics of cached triggers
       dm_exec_trigger_stats.cached_time  as cached_time,
       dm_exec_trigger_stats.last_execution_time  as last_execution_time,
       dm_exec_trigger_stats.execution_count      as execution_count,
       dm_exec_trigger_stats.total_worker_time    as total_worker_time,
       dm_exec_trigger_stats.last_worker_time     as last_worker_time,
       dm_exec_trigger_stats.min_worker_time      as min_worker_time,
       dm_exec_trigger_stats.max_worker_time      as max_worker_time,
       dm_exec_trigger_stats.total_elapsed_time   as total_elapsed_time,
       dm_exec_trigger_stats.last_elapsed_time    as last_elapsed_time,
       dm_exec_trigger_stats.min_elapsed_time     as min_elapsed_time,
       dm_exec_trigger_stats.max_elapsed_time     as max_elapsed_time,

       -- Page server reads  
       dm_exec_trigger_stats.total_page_server_reads  as total_page_server_reads,
       dm_exec_trigger_stats.last_page_server_reads   as last_page_server_reads,
       dm_exec_trigger_stats.min_page_server_reads    as min_page_server_reads,
       dm_exec_trigger_stats.max_page_server_reads    as max_page_server_reads,
       dm_exec_trigger_stats.total_num_page_server_reads  as total_num_page_server_reads,
       dm_exec_trigger_stats.last_num_page_server_reads   as last_num_page_server_reads,
       dm_exec_trigger_stats.min_num_page_server_reads    as min_num_page_server_reads,
       dm_exec_trigger_stats.max_num_page_server_reads    as max_num_page_server_reads,

       -- Logical I/O  
       dm_exec_trigger_stats.total_logical_writes as total_logical_writes,
       dm_exec_trigger_stats.last_logical_writes  as last_logical_writes,
       dm_exec_trigger_stats.min_logical_writes   as min_logical_writes,
       dm_exec_trigger_stats.max_logical_writes   as max_logical_writes,
       dm_exec_trigger_stats.total_logical_reads  as total_logical_reads,
       dm_exec_trigger_stats.last_logical_reads   as last_logical_reads,
       dm_exec_trigger_stats.min_logical_reads    as min_logical_reads,
       dm_exec_trigger_stats.max_logical_reads    as max_logical_reads

FROM sys.dm_exec_trigger_stats dm_exec_trigger_stats
LEFT JOIN sys.server_triggers server_triggers
   ON server_triggers.object_id = dm_exec_trigger_stats.object_id AND
      server_triggers.type = dm_exec_trigger_stats.type;
```
