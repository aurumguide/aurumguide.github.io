---
title: "How to use the procedure IF statement"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql procedure]

## permalink: /MsSql//////////

toc: true
toc_sticky: true
 
date: 2024-09-15
last_modified_at: 2024-09-15
---

MSSQL procedure IF statement is a reserved word used to process branches according to conditions. Other programming languages ​​may have differences in grammar, but the definition is the same.

## <mark>Procedure IF statement definition and explanation.</mark>

- The IF statement is the most basic concept in programming languages.
- The IF statement is used to process branches in procedures.
- IF statements can be used nested in procedures.
- Procedure IF statements can use multiple conditional statements.

![Procedure IF statement flow chart.](/assets/images/postsImages/MsSql/1048_Stored_Procedure_IF_statement/1.png)

## <mark>How to use the IF statement in the procedure.</mark>

### ***How to use the IF branch processing.***

- The procedure IF statement needs to have a begin and end block for better maintenance.

```sql
-- How to use the IF branch processing.
-- DROP PROCEDURE [STORE_PROCEDURE_IF_STATEMENT_EX1];
CREATE  PROCEDURE [DBO].[STORE_PROCEDURE_IF_STATEMENT_EX1]        
    @UseAge INT
AS
BEGIN
   IF(@UseAge = 20)   
   BEGIN
      PRINT('Print if UseAge is 20.');
   END
   ELSE IF (@UseAge = 30) 
   BEGIN
      PRINT('Print if UseAge is 30.');
   END
   ELSE 
   BEGIN 
     PRINT('Print if UseAge is not 20 or 30.');
   END
END;
```

### ***How to use nested IF statements in procedures.***

- You can use nested IF statements inside IF conditional statements.

```sql
-- How to use nested IF statements in procedures.
-- DROP PROCEDURE [[STORE_PROCEDURE_IF_STATEMENT_EX2]];
CREATE  PROCEDURE [DBO].[STORE_PROCEDURE_IF_STATEMENT_EX2]  
    @UserNm  VARCHAR(100),
    @UseAge INT
AS
BEGIN  
  IF(@UseAge = 20) 
  BEGIN
    IF(@UserNm ='Lee')
    BEGIN
     PRINT('Print if UseAge: 20, UserNm:Lee. ');
    END
    ELSE
    BEGIN
      PRINT('If UseAge: 20 and UserNm: not Lee, print');
    END 
  END
  ELSE
   BEGIN
     PRINT('If UseAge is not 20, print it.');
   END 
END;
```

### ***Procedure IF statement uses multiple conditional statements.***

- You can write multiple conditional statements using AND and OR reserved words in the IF conditional statement.

```sql
-- Procedure IF statement uses multiple conditional statements.
-- DROP PROCEDURE [STORE_PROCEDURE_IF_STATEMENT_EX3];
CREATE  PROCEDURE [DBO].[STORE_PROCEDURE_IF_STATEMENT_EX3]  
   @UserNm  VARCHAR(100),
   @UseAge INT,
   @UserAddress  VARCHAR(100)
AS
BEGIN
   IF(@UseAge IN('10','20','30') AND  @UserNm = 'Lee' and @UserAddress = 'LA' ) 
   BEGIN
     PRINT('If UseAge: 10, 20, 30 are included, UserNm: Lee, and UserAddress: LA, print it. ');
   END
   ELSE
   BEGIN
     PRINT('If UseAge: 10, 20, 30 is included, UserNm: Lee, and UserAddress: is not LA, print it. ');
   END
END;
```

## <mark>Ending the procedure IF statement.</mark>

- The case when statement can also be used for branching with the if statement.
- In T-SQL, you can use the IF statement by using the declare statement.
- In MSSQL, you can also use conditions such as between, in, and not.
