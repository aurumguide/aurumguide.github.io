---
title: "How to use MSSQL function IF, WHILE statement"
excerpt: ""

categories:
  - MsSql
tags:
  - [MsSql Function]

## permalink: /MsSql//////////

toc: true
toc_sticky: true
 
date: 2024-09-29
last_modified_at: 2024-09-29
---
 MSSQL functions also support basic IF, WHILE, but inline table-valued functions do not support them.

## <mark>MSSQL function IF, WHILE description.</mark>

- There is a question about whether control statements can be used in MSSQL functions, and the conclusion is that they can be used.
- However, they are not possible when using inline table-valued functions.
- Of course, declear variable declarations cannot be used either.
- They are possible when using scalar functions and multi-statement table-returning functions.

## <mark>MSSQL function IF, WHILE statement usage and examples.</mark>

### ***IF, WHILE usage examples.***

- Codes and error codes that can be used with IF, WHILE.

![Here are some examples of using IF and WHILE.](/assets/images/postsImages/MsSql/1055_function_if_WHILE/function_if_while.jpg)

### ***How to use Scala function IF, WHILE statement.***

- Scala function source code.

```sql
CREATE FUNCTION AurumGuide_ControlFunc1 (
  @PARAM1 int 
)
RETURNS int
AS
BEGIN
declare @AurumNum int,
@returnStr int;
    SET @returnStr = 0;
   SET @AurumNum  = 1;

    WHILE @AurumNum < 15
    BEGIN
       IF(@AurumNum < 10) 
    BEGIN
        set @returnStr +=  @PARAM1;
    END
    ELSE
    BEGIN
       set @returnStr  += -1;
    END
       SET @AurumNum = @AurumNum +  1;
    END;

RETURN @returnStr
END;
-- EXCUTE
SELECT DBO.AurumGuide_ControlFunc1(2);
```

### ***How to use the inline table return function IF, WHILE statement.***

- This is the source code for the inline table return function.

```sql
CREATE FUNCTION DBO.fn_AurumGuide_ControlFunc2
(
    @AurumId INT
)
RETURNS TABLE
as 
RETURN 
(  
    WHILE @AurumId < 4
    BEGIN
        -- INSERT @Return_AurumGuide
        SELECT @AurumId ;

        SET @AurumId = @AurumId + 1;
    END;
);
```

### ***How to use the IF, WHILE statement for returning a multi-sentence table function.***

- This is the source code for the multi-sentence table return function.

```sql
CREATE FUNCTION DBO.fn_AurumGuide_ControlFunc3  (@AurumId int)
RETURNS @Return_AurumGuide  TABLE
(
    AurumId  int,
    AurumNm varchar(255) NULL 
) AS
BEGIN
    DECLARE @NUM INT;

    SET @NUM = 1
   
    WHILE @NUM < 4
    BEGIN
    IF(@AurumId = '')
    BEGIN
        INSERT @Return_AurumGuide
        SELECT AurumId,AurumNm
          FROM AurumGuide_ControlFunc af;
    END 
    ELSE 
    BEGIN
        INSERT @Return_AurumGuide
        SELECT AurumId,AurumNm
          FROM AurumGuide_ControlFunc af
         WHERE af.AurumId  =  @AurumId ;
    END;
SET @NUM += 1;
    END;
    RETURN;
END;
 
-- EXCUTE
SELECT * FROM DBO.fn_AurumGuide_ControlFunc3(3);
```

## <mark>When to use inline table-valued functions?</mark>

- It is easy to understand if you think that only simple SELECT clauses can be placed in the RETURN clause.
- Normally, the returned column is fixed, but Inline table-valued functions can return data without fixing it.
