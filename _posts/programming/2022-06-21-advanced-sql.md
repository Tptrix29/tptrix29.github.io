---
title:  "Advanced SQL"
date:   2022-06-21 -0500
author: Pei Tian
categories: programming
tags: SQL 
header:
    teaser: /assets/img/training-guidence.png
---

## Advanced SQL Syntax

### Mathematic Functions

```sql
 ABS(x)
 CEIL(x)
 FLOOR(x)
 RAND()  -- random number in [0, 1]
 RAND(x)
 SIGN(x)  -- return -1/0/1
 PI()
 TRUNCATE(x, y)  -- reserve numbers after point with y count
 ROUND(x)  -- reserve as integer
 ROUND(x, y)
 POW(x, y)
 SQRT(x)
 EXP(x)
 MOD(x, y)  -- remainder
 LOG(x)
 LOG10(x)
 RADIANS(x)
 DEGREES(x)
 SIN(x)
 COS(x)
 TAN(x)
 ASIN(x)
 ACOS(x)
 ATAN(s)
```

### String Functions

```sql
 CHAR_LENGTH(s)  -- char count
 LENGTH(s)  -- length
 CONCAT(s1, ...)
 CONCAT(x, s1, ...)  -- "x".join([s1, s2, ...])
 INSERT(s1, x, len, s2)
 -- SELECT INSERT('12345', 1, 3, 'abc')
 -- [OUT]: abc45
 REPLACE(s, s1, s2)  -- replace s1 in s with s2
 UPPER(s)
 LOWER(s)
 LEFT(s, n)
 RIGHT(s, n)
 TRIM(s)
 LTRIM(s)
 RTRIM(s)
 REPEAT(s, n)
 SUBSTRING(s, n, len)
 INSTR(s, s1)  -- get the start location of s in s1
 REVERSE(s)
```

### Time Functions

```sql
 NOW()
 FORMAT(NOW(), "%Y-%m-%d")
 
 DATEDIFF(d1, d2)  -- d1 - d2
 TIMEDIFF(t1, t2)
 
 ADDDATE(d, n)
 ADDDATE(d, INTERVAL n TYPE)
 -- SELECT ADDDATE(CURRENT(), INTERVAL 3 MONTH)
 SUBDATE(d, n)
 SUBDATE(d, INTERVAL n TYPE)
```

### Aggrevgative Functions

```sql
 AVG([colname])
 SUM([colname])
 COUNT([colname])
 MAX([colname])
 MIN([colname])
```

### Conditional Clause

```sql
 -- IF-ELSE
 IF([cond], value1, value2)
 
 -- CASE
 CASE
   WHEN [cond1] THEN ...
   WHEN [cond2] THEN
   ELSE ...
 END
```

```sql
-- Example
-- Determine the income level of employees
SELECT 
	salary,
	CASE 
		WHEN income > 100000 THEN "high"
		WHEN income BETWEEN 50000 AND 100000 THEN "medium"
		ELSE "low"
	END as income_level
FROM Employee
```



### WITH

```sql
 WITH t AS (
   SELECT * FROM UTable
 )
```

### Encryption

```sql
 PASSWORD(s)
 MD5(s)
 ENCODE(str, pswd_str)
 DECODE(crypt_str, pswd_str)
```



## Window Function

### Definition

```sql
 func(expr) OVER (
   PARTITION BY ...
   ORDER BY ...
   { ROWS | RANGE } frame_start /
   { ROWS | RANGE } BETWEEN frame_start AND frame_end
 )
```

### Frame Clause

```sql
 RANGE -- value range
 ROWS -- row index range
```

```sql
 CURRENT ROW  -- current row
 UNBOUNDED PRECEDING  -- first row in partition
 UNBOUNDED FOLLOWING  -- last row in partition
 n PRECEDING  -- n-th preceding row of current row
 n FOLLOWING  -- n-th following row of current row
```

### Useful Functions

### Aggregative Functions

```sql
 -- As metioned above in 'Aggregative Funtions'
```

### Ranking Functions

```sql
 ROW_NUMBER()
 RANK()
 DENSE_RANK()
 PERCENT_RANK()  -- ranking in percent
 CUME_DIST()  -- culmulative distribution
```

### Value Functions

```sql
 FIRST_VALUE([colname])  -- first value of partition
 LAST_VALUE([colname])  -- last value partition
 NTH_VALUE([colname], n)  -- n-th value in partition
 LAG([colname], n)  -- n-th value after current row
 LEAD([colname], n)  -- n-th value before current row
```

## Regexp

```SQL
SELECT * FROM [table] WHERE [VAR] REGEXP '^\d-';
```

