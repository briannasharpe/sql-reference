# SQL Reference

* **MySQL** (current)
* [PostgreSQL](postgresql.md)

1. [📁 Query](#query)  
  • [📂 Misc](#misc)  
  &nbsp; ∘ [Common Table Expression (CTE)](#common-table-expression)  
  &nbsp; ∘ [Operator](#operator)  
  &nbsp; ∘ [Expression](#expression)  
  &nbsp; ∘ [Wildcards](#wildcards)  
  &nbsp; ∘ [Commands](#commands)  
2. [🛠️ Functions](#functions)  
  • [📊 Aggregate Functions](#aggregate-functions)  
  • [🔍 Window Functions](#window-functions)  
  • [🛠️ Misc Functions](#misc-functions)  
  &nbsp; ∘ [⛔ Null](#null)  
  &nbsp; ∘ [❔ Conditional Branching](#conditional-branching)  
  &nbsp; ∘ [📐 Math](#math)  
  &nbsp; ∘ [💬 String](#string)  
  &nbsp; ∘ [📆 Date](#date)  
3. [🗃️ Database](#database)  
  • [General](#general)  
  • [Stored Functions and Procedures](#stored-functions-and-procedures)  
  • [Import Spreadsheet](#import-spreadsheet)
<!-- 4. [MySQL Workbench](#mysql-workbench) -->

<!-- ----------------------------------------------------------------------- -->

## QUERY

```sql
SELECT col1, AGG(col2), ...
FROM table1 t1
JOIN table2  t2
  ON t1.id = t2.id
WHERE <condition>
GROUP BY ...
HAVING <condition>
ORDER BY ...
LIMIT <#>
```

1. [Select](#1-select)  
2. [Join](#2-join)  
3. [Where](#3-where)  
4. [Group By](#4-group-by)  
5. [Having](#5-having)  
6. [Order By](#6-order-by)  
7. [Limit](#7-limit)
8. [Misc](#misc)  
  • [Common Table Expression (CTE)](#common-table-expression)  
  • [Operator](#operator)  
  • [Expression](#expression)  
  • [Wildcards](#wildcards)

<!-- ----------------------------------------------------------------------- -->

### 1. SELECT

```sql
DISTINCT
```

<!-- ----------------------------------------------------------------------- -->

### 2. JOIN

```sql
JOIN -- INNER
LEFT JOIN
RIGHT JOIN
FULL JOIN -- OUTER
CROSS JOIN
```

* `JOIN` - matching rows  
* `FULL JOIN` - all rows  
* `CROSS JOIN` - match each row (cartesian product)  

```sql
-- MySQL FULL JOIN
SELECT * FROM a
LEFT JOIN b ON a.id = b.id
UNION
SELECT * FROM a
RIGHT JOIN b ON a.id = b.id
```

```sql
JOIN ... USING (column_of_same_name) -- instead of ON
```

```sql
JOIN ... 
ON ... LIKE ...
```

<!-- ----------------------------------------------------------------------- -->

### 3. WHERE

* filter before aggregation

```sql
WHERE NOT ...
```

```sql
WHERE ... OR ...
```

```sql
WHERE ... AND ...
WHERE ... AND (... OR ...)
```

```sql
WHERE ... BETWEEN ... AND ...
```

```sql
WHERE ... IN ...
WHERE ... IN ('value1', 'value2', ...)
WHERE ... IN <SELECT statement>

WHERE col IN ... OR col = NULL -- for NULL values

WHERE ... NOT IN ...
WHERE ... NOT IN <SELECT statement>
```

* `IN` matches any value in a list of specified values
  * shorthand for multiple `OR` conditions
  * `WHERE ... IN <value1, value2, ...>` = `WHERE ... <value1> OR <value2> OR <value3>`
* `WHERE ... IN <SELECT statement>` must use the same number of columns

```sql
WHERE ... LIKE '<pattern>'
WHERE ... NOT LIKE '<pattern>'

-- regular expressions
WHERE ... REGEXP '<pattern>'
WHERE ... NOT REGEXP '<pattern>'

-- example
WHERE mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode[.]com$'
```

* see [WILDCARDS](#wildcards)

```sql
WHERE EXISTS <SELECT statement>
```

<!--
* NOT IN =/= NOT EXISTS (with NULL) https://stackoverflow.com/questions/173041/not-in-vs-not-exists
-->

<!-- ----------------------------------------------------------------------- -->

### 4. GROUP BY

```sql
GROUP BY <#>
```

* `#` is based on `SELECT` statement
* ex. `GROUP BY 1` = first column in `SELECT` list

<!-- ----------------------------------------------------------------------- -->

### 5. HAVING

* filter after aggregation

```sql
HAVING ... = <SELECT statement>
```

<!-- ----------------------------------------------------------------------- -->

### 6. ORDER BY

```sql
ORDER BY ... ASC -- default
ORDER BY ... DESC
ORDER BY ..., ...
```

```sql
ORDER BY <#>
```

* `#` is based on `SELECT` statement
* ex. `ORDER BY 1` = first column in `SELECT` list

```sql
ORDER BY <expression> -- expression or function
```

```sql
ORDER BY ... NULLS FIRST; -- control sort order of NULLS
```

```sql
ORDER BY 
  CASE
    WHEN ... THEN ...
  END
```

<!-- ----------------------------------------------------------------------- -->

### 7. LIMIT

```sql
LIMIT A OFFSET B; -- skip top B rows and fetch the next A
```

<!-- https://www.datacamp.com/tutorial/sql-offset -->

<!-- ----------------------------------------------------------------------- -->

### MISC

1. [Common Table Expression (CTE)](#common-table-expression)
2. [Operator](#operator)  
3. [Expression](#expression)  
4. [Wildcards](#wildcards)  
5. [Commands](#commands)  

#### COMMON TABLE EXPRESSION

```sql
WITH cte_name AS (
  SELECT ...
)

SELECT *
FROM cte_name;
```

<!-- ----------------------------------------------------------------------- -->

#### OPERATOR

> UNION

```sql
<SELECT statement>
UNION
<SELECT statement>
```

* combine result-set of two or more `SELECT` statements
* must have same number of columns and similar data types
* `UNION ALL` allow duplicate values

> NULL

```sql
IS NULL
IS NOT NULL
```

* see [`ISNULL()`](#null), [`IFNULL()`](#null)

<!-- ----------------------------------------------------------------------- -->

#### EXPRESSION

> CASE

* basically if/then logic
* see function [`IF()`](#conditional-branching)
* no `ELSE` will act as an `ELSE NULL` clause

```sql
CASE
  ...
END AS '<alias>'
```

```sql
CASE
  ...
END
```

> Simple CASE

```sql
CASE <expression>
  WHEN 'value' THEN 'result'
    ...
  WHEN 'value' THEN 'result'
  ELSE 'result'
END
```

* `<expression>` can be:
  * column (`column_name`)
  * literal value (`'string'`)
  * constant (predefined known value)
  * calulated value (`price - discount`, `salary * 2`)
  * function result (`MONTH(order_date)`)
  * subquery result (`SELECT COUNT(*) FROM ...`)
  * type-cast value (`CAST(value AS INT)`)
  * `NULL`

> Searched CASE

```sql
CASE
  WHEN <condition> THEN 'result'
    ...
  WHEN <condition> THEN 'result'
  ELSE 'result'
END
```

```sql
CASE
  WHEN <condition> IN ('value', 'value') THEN 'result'
END

CASE
  WHEN column LIKE '[ABC]%' THEN 'result'
END
```

<!-- ----------------------------------------------------------------------- -->

#### WILDCARDS

> % - any number of characters

```sql
'startswith%'
'%endswith'
'%contains%'
'[a-f]%' -- any starting with a, b, c, d, e, or f
'[bkp]%' -- any starting with b, k, or p
```

* `-` - range
* `[]` - any single character within brackets

> _ - single character

```sql
'_anyfollowedby'
```

* `'_ondon'` - any character followed by 'ondon'
* `'L___on'` - start with L, followed by any 3 characters, ending with 'on'

> COMBINED

```sql
'a__%' -- start with a, at least 3 characters in length
'_r%' -- any with r in second position
```

> REGULAR EXPRESSIONS (`REGEXP`)

```sql
'^[a-z]' -- start with a-z
'[a-z]$' -- end with a-z
'[a-z]*' -- any sequence of a-z
```

* `^` - match beginning of string
* `$` - match end of string
* `.` - matches any single character, including newlines
* `*` - match any sequence of zero or more characters

<!-- ----------------------------------------------------------------------- -->

#### COMMANDS

```sql
DELIMITER $$
DELIMITER //
DELIMITER ;
```

<!-- ----------------------------------------------------------------------- -->

## FUNCTIONS

1. [📊 Aggregate Functions](#aggregate-functions)  
2. [🔍 Window Functions](#window-functions)  
3. [🛠️ Misc Functions](#misc-functions)  
  • [⛔ Null](#null)  
  • [❔ Conditional Branching](#conditional-branching)  
  • [📐 Math](#math)  
  • [💬 String](#string)  
  • [📆 Date](#date)  

<!-- ----------------------------------------------------------------------- -->

### AGGREGATE FUNCTIONS

```sql
SUM()
```

* `SUM(<condition>)` - the count of rows where the condition is TRUE  
* `SUM(CASE WHEN <condition> THEN col ELSE 0 END)` - sum of condition <!-- (see [`CASE`](#case)) --> (preferred)
* `SUM(<condition> * col)` - sum of condition <!-- ? --> (if condition is `NULL`, the row is ignored)
* rolling total - see [WINDOW FUNCTIONS](#window-functions)

```sql
AVG()
```

```sql
MIN()
```

```sql
MAX()
```

* `MAX(varchar)` - based on lexicographical order (ASCII value)

```sql
COUNT()
```

* `COUNT(*)` - all rows (`NULL` rows counted)  
* `COUNT(col)` - non-NULL rows (`NULL` rows <ins>**not**</ins> counted)  

<!-- ----------------------------------------------------------------------- -->

### WINDOW FUNCTIONS

* calculations across a specified set of rows related to the current row (within a defined "window" of data)
* does not alter the data set

```sql
<window function>() OVER()
<window function>() OVER(PARTITION BY ... )
<window function>() OVER(ORDER BY ...)
<window function>() OVER(PARTITION BY ... ORDER BY ...)
<window function>() OVER(PARTITION BY ... ORDER BY ...) AS <alias>
```

* `<window function>(<column>)` - column to apply window function to

```sql
OVER(ROWS BETWEEN <lower_bound> AND <upper_bound>) -- number of rows
OVER(RANGE BETWEEN <lower_bound> AND <upper_bound>) -- values
```

* bounds can be:
  * `UNBOUNDED PRECEDING` – all rows before the current row
  * `# PRECEDING` – # rows before the current row
  * `CURRENT ROW` – just the current row
  * `# FOLLOWING` – # rows after the current row
  * `UNBOUNDED FOLLOWING` – all rows after the current row
* `RANGE` - typically used with `date` or `timestamp` values

> RANKING

```sql
ROW_NUMBER()
```

* `ROW_NUMBER()` - unique sequential number

```sql
RANK()
DENSE_RANK()
```

* duplicates are based on `ORDER BY` and assigns the same number
* `RANK()` - not numerically but positionally (ex. 4 5 5 7)
* `DENSE_RANK()` - numerically (ex. 4 5 5 6)

> POSITION

```sql
LAG(expression, offset, default) -- previous row
```

```sql
LEAD(expression, offset, default) -- next row
```

* `expression` - column or built-in function
* `offset` - physical offset from current row (default is 1)
* `default` - value when offset goes beyond partition scope (default is `NULL`)

```sql
FIRST_VALUE(expression)
LAST_VALUE(expression)
NTH_VALUE(expression, #)
```

> ROLLING TOTAL

```sql
SUM(...) OVER(...)
```

```sql
COUNT(...) OVER(...)
```

```sql
-- self join version
SELECT *
FROM a
JOIN b
  ON a.id_column >= b.id_column
GROUP BY a.id_column
HAVING SUM(value_column) <= <#>
```

```sql
-- date range
SUM(...) OVER(RANGE BETWEEN INTERVAL <value> <unit> PRECEDING AND CURRENT ROW)
```

* see [`DATE_ADD` and `DATE_SUB` parameter values](#date)

<!-- ----------------------------------------------------------------------- -->

### MISC FUNCTIONS

<!-- ----------------------------------------------------------------------- -->

#### NULL

```sql
ISNULL(expression)
```

```sql
IFNULL(expression, alt_value)
```

* `expression` - expression to test
* `alt_value` - return value if expression is NULL

<!-- ----------------------------------------------------------------------- -->

#### CONDITIONAL BRANCHING

```sql
IF(condition, value_if_true, value_if_false)
```

<!-- ----------------------------------------------------------------------- -->

#### MATH

```sql
ROUND(number, decimal_value, )
```

```sql
MOD(x, y) -- %
```

<!-- ----------------------------------------------------------------------- -->

#### STRING

```sql
LENGTH(string) -- in bytes
CHAR_LENGTH(string) -- in characters
```

```sql
CONCAT(string, string, ...)
```

* add strings together

```sql
GROUP_CONCAT(column ORDER BY ... SEPARATOR separator_value) -- ORDER BY and SEPARATOR are optional
GROUP_CONCAT(DISTINCT column ...)
```

* add values from multiple rows as a group
* separator default is comma

```sql
LOWER(string) -- lowercase
UPPER(string) -- UPPERCASE
```

```sql
LEFT(string, #)
RIGHT(string, #)
```

* extract number of characters from a string

```sql
SUBSTRING(string, start, length) -- length is optional, returns whole string if not specified
```

* `start` at position #, extract `length` characters

```sql
SUBSTRING_INDEX(string, delimiter, number)
```

* `delimiter` - the mark to search for
* `number` - number of times to search delimiter
  * positive number = all left of delimiter
  * negative number = all right of delimiter

```sql
REPLACE(string, old_string, new_string)
```

* replace all occurences of a substring within a string

```sql
REGEXP_REPLACE(expression, pattern, replacement)

-- optional: position, occurence, match_type
REGEXP_REPLACE(expression, pattern, replacement, position, occurence, match_type)
```

* replace substrings matching regular expression
* `position` - position in `expression` to start the search (default is 1)
* `occurence` - which occurence of a match to replace (default is 0, replace all occurences)
* `match_type` - string that specifies how to perform matching

<!-- 

REGEXP_LIKE
REGEXP_REPLACE
REGEXP_SUBSTR

https://dev.mysql.com/doc/refman/8.4/en/regexp.html

NOT REGEXP	Negation of REGEXP
REGEXP	Whether string matches regular expression
REGEXP_INSTR()	Starting index of substring matching regular expression
REGEXP_LIKE()	Whether string matches regular expression
REGEXP_REPLACE()	Replace substrings matching regular expression
REGEXP_SUBSTR()	Return substring matching regular expression
RLIKE	Whether string matches regular expression

https://dev.mysql.com/doc/refman/8.0/en/regexp.html#function_regexp-substr

-->

<!-- ----------------------------------------------------------------------- -->

#### DATE

```sql
DAY()
MONTH()
YEAR()
```

```sql
EXTRACT(part FROM date_value)
```

> `EXTRACT` parameter values ([source](https://www.w3schools.com/sql/func_mysql_extract.asp))

| Parameter  | Description                                                                                                              |
| ---------- | ------------------------------------------------------------------------------------------------------------------------ |
| part       | Required. The part to extract. Can be one of the following:<li>MICROSECOND</li> <li>SECOND</li> <li>MINUTE</li> <li>HOUR</li> <li>DAY</li> <li>WEEK</li> <li>MONTH</li> <li>QUARTER</li> <li>YEAR</li> <li>SECOND_MICROSECOND</li> <li>MINUTE_MICROSECOND</li> <li>MINUTE_SECOND</li> <li>HOUR_MICROSECOND</li> <li>HOUR_SECOND</li> <li>HOUR_MINUTE</li> <li>DAY_MICROSECOND</li> <li>DAY_SECOND</li> <li>DAY_MINUTE</li> <li>DAY_HOUR</li> <li>YEAR_MONTH</li>                                                                                                   |
| date_value | Required. The date to extract a part from                                                                                |

<!-- ! EXTRACT() table

| Parameter  | Description                                                                                                              |
| ---------- | ------------------------------------------------------------------------------------------------------------------------ |
| part       | Required. The part to extract. Can be one of the following:<li>MICROSECOND</li> <li>SECOND</li> <li>MINUTE</li> <li>HOUR</li> <li>DAY</li> <li>WEEK</li> <li>MONTH</li> <li>QUARTER</li> <li>YEAR</li> <li>SECOND_MICROSECOND</li> <li>MINUTE_MICROSECOND</li> <li>MINUTE_SECOND</li> <li>HOUR_MICROSECOND</li> <li>HOUR_SECOND</li> <li>HOUR_MINUTE</li> <li>DAY_MICROSECOND</li> <li>DAY_SECOND</li> <li>DAY_MINUTE</li> <li>DAY_HOUR</li> <li>YEAR_MONTH</li>                                                                                                   |
| date_value | Required. The date to extract a part from                                                                                |

-->

```sql
DATEDIFF(date1, date2)
```

```sql
DATE_FORMAT(col, '<format>')
```

> `DATE_FORMAT` parameter values ([source](https://www.w3schools.com/sql/func_mysql_date_format.asp))

| Format | Description                                                                  |
| ------ | ---------------------------------------------------------------------------- |
| %a     | Abbreviated weekday name (Sun to Sat)                                        |
| %b     | Abbreviated month name (Jan to Dec)                                          |
| %c     | Numeric month name (0 to 12)                                                 |
| %D     | Day of the month as a numeric value, followed by suffix (1st, 2nd, 3rd, ...) |
| %d     | Day of the month as a numeric value (01 to 31)                               |
| %e     | Day of the month as a numeric value (0 to 31)                                |
| %f     | Microseconds (000000 to 999999)                                              |
| %H     | Hour (00 to 23)                                                              |
| %h     | Hour (00 to 12)                                                              |
| %I     | Hour (00 to 12)                                                              |
| %i     | Minutes (00 to 59)                                                           |
| %j     | Day of the year (001 to 366)                                                 |
| %k     | Hour (0 to 23)                                                               |
| %l     | Hour (1 to 12)                                                               |
| %M     | Month name in full (January to December)                                     |
| %m     | Month name as a numeric value (00 to 12)                                     |
| %p     | AM or PM                                                                     |
| %r     | Time in 12 hour AM or PM format (hh:mm:ss AM/PM)                             |
| %S     | Seconds (00 to 59)                                                           |
| %s     | Seconds (00 to 59)                                                           |
| %T     | Time in 24 hour format (hh:mm:ss)                                            |
| %U     | Week where Sunday is the first day of the week (00 to 53)                    |
| %u     | Week where Monday is the first day of the week (00 to 53)                    |
| %V     | Week where Sunday is the first day of the week (01 to 53). Used with %X      |
| %v     | Week where Monday is the first day of the week (01 to 53). Used with %x      |
| %W     | Weekday name in full (Sunday to Saturday)                                    |
| %w     | Day of the week where Sunday=0 and Saturday=6                                |
| %X     | Year for the week where Sunday is the first day of the week. Used with %V    |
| %x     | Year for the week where Monday is the first day of the week. Used with %v    |
| %Y     | Year as a numeric, 4-digit value                                             |
| %y     | Year as a numeric, 2-digit value                                             |

<!-- ! DATE_FORMAT table

| Format | Description                                                                  |
| ------ | ---------------------------------------------------------------------------- |
| %a     | Abbreviated weekday name (Sun to Sat)                                        |
| %b     | Abbreviated month name (Jan to Dec)                                          |
| %c     | Numeric month name (0 to 12)                                                 |
| %D     | Day of the month as a numeric value, followed by suffix (1st, 2nd, 3rd, ...) |
| %d     | Day of the month as a numeric value (01 to 31)                               |
| %e     | Day of the month as a numeric value (0 to 31)                                |
| %f     | Microseconds (000000 to 999999)                                              |
| %H     | Hour (00 to 23)                                                              |
| %h     | Hour (00 to 12)                                                              |
| %I     | Hour (00 to 12)                                                              |
| %i     | Minutes (00 to 59)                                                           |
| %j     | Day of the year (001 to 366)                                                 |
| %k     | Hour (0 to 23)                                                               |
| %l     | Hour (1 to 12)                                                               |
| %M     | Month name in full (January to December)                                     |
| %m     | Month name as a numeric value (00 to 12)                                     |
| %p     | AM or PM                                                                     |
| %r     | Time in 12 hour AM or PM format (hh:mm:ss AM/PM)                             |
| %S     | Seconds (00 to 59)                                                           |
| %s     | Seconds (00 to 59)                                                           |
| %T     | Time in 24 hour format (hh:mm:ss)                                            |
| %U     | Week where Sunday is the first day of the week (00 to 53)                    |
| %u     | Week where Monday is the first day of the week (00 to 53)                    |
| %V     | Week where Sunday is the first day of the week (01 to 53). Used with %X      |
| %v     | Week where Monday is the first day of the week (01 to 53). Used with %x      |
| %W     | Weekday name in full (Sunday to Saturday)                                    |
| %w     | Day of the week where Sunday=0 and Saturday=6                                |
| %X     | Year for the week where Sunday is the first day of the week. Used with %V    |
| %x     | Year for the week where Monday is the first day of the week. Used with %v    |
| %Y     | Year as a numeric, 4-digit value                                             |
| %y     | Year as a numeric, 2-digit value                                             |

-->

```sql
DATE_ADD(date, INTERVAL <value> <unit>)
DATE_SUB(date, INTERVAL <value> <unit>)
```

> `DATE_ADD` and `DATE_SUB` parameter values ([`ADD` source](https://www.w3schools.com/sql/func_mysql_date_add.asp), [`SUB` source](https://www.w3schools.com/sql/func_mysql_date_sub.asp))

| Parameter | Description                                                                                                              |
| --------- | ------------------------------------------------------------------------------------------------------------------------ |
| date      | Required. The date to be modified                                                                                        |
| value     | Required. The value of the time/date interval to add/subtract. Both positive and negative values are allowed             |
| unit      | Required. The type of interval to add/subtract. Can be one of the following values:<li>MICROSECOND</li> <li>SECOND</li> <li>MINUTE</li> <li>HOUR</li> <li>DAY</li> <li>WEEK</li> <li>MONTH</li> <li>QUARTER</li> <li>YEAR</li> <li>SECOND_MICROSECOND</li> <li>MINUTE_MICROSECOND</li> <li>MINUTE_SECOND</li> <li>HOUR_MICROSECOND</li> <li>HOUR_SECOND</li> <li>HOUR_MINUTE</li> <li>DAY_MICROSECOND</li> <li>DAY_SECOND</li> <li>DAY_MINUTE</li> <li>DAY_HOUR</li> <li>YEAR_MONTH</li>                                                          |

<!-- ! DATE_ADD / DATE_SUB table

| Parameter | Description                                                                                                              |
| --------- | ------------------------------------------------------------------------------------------------------------------------ |
| date      | Required. The date to be modified                                                                                        |
| value     | Required. The value of the time/date interval to add/subtract. Both positive and negative values are allowed             |
| unit      | Required. The type of interval to add/subtract. Can be one of the following values:<li>MICROSECOND</li> <li>SECOND</li> <li>MINUTE</li> <li>HOUR</li> <li>DAY</li> <li>WEEK</li> <li>MONTH</li> <li>QUARTER</li> <li>YEAR</li> <li>SECOND_MICROSECOND</li> <li>MINUTE_MICROSECOND</li> <li>MINUTE_SECOND</li> <li>HOUR_MICROSECOND</li> <li>HOUR_SECOND</li> <li>HOUR_MINUTE</li> <li>DAY_MICROSECOND</li> <li>DAY_SECOND</li> <li>DAY_MINUTE</li> <li>DAY_HOUR</li> <li>YEAR_MONTH</li>                                                          |

-->

<!-- ----------------------------------------------------------------------- -->

## DATABASE

1. [General](#general)  
  • [Index](#index)  
2. [Stored Functions and Procedures](#stored-functions-and-procedures)  
3. [Import Spreadsheet](#import-spreadsheet)

<!-- ----------------------------------------------------------------------- -->

### GENERAL

```sql
CREATE DATABASE database_name
DROP DATABASE database_name
```

```sql
CREATE TABLE table_name (
  column_name datatype,
  ...
  column_name datatype,
  PRIMARY KEY (column, column),
  INDEX idx_name (column, column)
)
```

```sql
CREATE TEMPORARY TABLE temp_table_name AS
  <sql_statement>
```

> STRING DATA TYPE parameter values ([source](https://www.w3schools.com/sql/sql_datatypes.asp))

| String Data Type            | Description                                                                                                                                                                        |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CHAR(size)                  | A FIXED length string (can contain letters, numbers, and special characters). The *size* parameter specifies the column length in characters - can be from 0 to 255. Default is 1  |
| VARCHAR(size)               | A VARIABLE length string (can contain letters, numbers, and special characters). The *size* parameter specifies the maximum string length in characters - can be from 0 to 65535   |
| BINARY(size)                | Equal to CHAR(), but stores binary byte strings. The *size* parameter specifies the column length in bytes. Default is 1                                                           |
| VARBINARY(size)             | Equal to VARCHAR(), but stores binary byte strings. The *size* parameter specifies the maximum column length in                                                                    |
| TINYBLOB                    | For BLOBs (Binary Large Objects). Max length: 255 bytes                                                                                                                            |
| TINYTEXT                    | Holds a string with a maximum length of 255 characters                                                                                                                             |
| TEXT(size)                  | Holds a string with a maximum length of 65,535 bytes                                                                                                                               |
| BLOB(size)                  | For BLOBs (Binary Large Objects). Holds up to 65,535 bytes of data                                                                                                                 |
| MEDIUMTEXT                  | Holds a string with a maximum length of 16,777,215 characters                                                                                                                      |
| MEDIUMBLOB                  | For BLOBs (Binary Large Objects). Holds up to 16,777,215 bytes of data                                                                                                             |
| LONGTEXT                    | Holds a string with a maximum length of 4,294,967,295 characters                                                                                                                   |
| LONGBLOB                    | For BLOBs (Binary Large Objects). Holds up to 4,294,967,295 bytes of data                                                                                                          |
| ENUM(val1, val2, val3, ...) | A string object that can have only one value, chosen from a list of possible values. You can list up to 65535 values in an ENUM list. If a value is inserted that is not in the list, a blank value will be inserted. The values are sorted in the order you enter them                                                                                                                                  |
| SET(val1, val2, val3, ...)  | A string object that can have 0 or more values, chosen from a list of possible values. You can list up to 64 values in a SET list                                                  |

<!-- ! string data type table

| String Data Type            | Description                                                                                                                                                                        |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CHAR(size)                  | A FIXED length string (can contain letters, numbers, and special characters). The *size* parameter specifies the column length in characters - can be from 0 to 255. Default is 1  |
| VARCHAR(size)               | A VARIABLE length string (can contain letters, numbers, and special characters). The *size* parameter specifies the maximum string length in characters - can be from 0 to 65535   |
| BINARY(size)                | Equal to CHAR(), but stores binary byte strings. The *size* parameter specifies the column length in bytes. Default is 1                                                           |
| VARBINARY(size)             | Equal to VARCHAR(), but stores binary byte strings. The *size* parameter specifies the maximum column length in                                                                    |
| TINYBLOB                    | For BLOBs (Binary Large Objects). Max length: 255 bytes                                                                                                                            |
| TINYTEXT                    | Holds a string with a maximum length of 255 characters                                                                                                                             |
| TEXT(size)                  | Holds a string with a maximum length of 65,535 bytes                                                                                                                               |
| BLOB(size)                  | For BLOBs (Binary Large Objects). Holds up to 65,535 bytes of data                                                                                                                 |
| MEDIUMTEXT                  | Holds a string with a maximum length of 16,777,215 characters                                                                                                                      |
| MEDIUMBLOB                  | For BLOBs (Binary Large Objects). Holds up to 16,777,215 bytes of data                                                                                                             |
| LONGTEXT                    | Holds a string with a maximum length of 4,294,967,295 characters                                                                                                                   |
| LONGBLOB                    | For BLOBs (Binary Large Objects). Holds up to 4,294,967,295 bytes of data                                                                                                          |
| ENUM(val1, val2, val3, ...) | A string object that can have only one value, chosen from a list of possible values. You can list up to 65535 values in an ENUM list. If a value is inserted that is not in the list, a blank value will be inserted. The values are sorted in the order you enter them                                                                                                                                  |
| SET(val1, val2, val3, ...)  | A string object that can have 0 or more values, chosen from a list of possible values. You can list up to 64 values in a SET list                                                  |

-->

> NUMERIC DATA TYPE parameter values ([source](https://www.w3schools.com/sql/sql_datatypes.asp))

| Numeric Data Type         | Description                                                                                                                                                                         |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| BIT(size)                 | A bit-value type. The number of bits per value is specified in*size* . The *size* parameter can hold a value from 1 to 64. The default value for *size* is 1.                       |
| TINYINT(size)             | A very small integer. Signed range is from -128 to 127. Unsigned range is from 0 to 255. The *size* parameter specifies the maximum display width (which is 255)                    |
| BOOL                      | Zero is considered as false, nonzero values are considered as true.                                                                                                                 |
| BOOLEAN                   | Equal to BOOL                                                                                                                                                                       |
| SMALLINT(size)            | A small integer. Signed range is from -32768 to 32767. Unsigned range is from 0 to 65535. The *size* parameter specifies the maximum display width (which is 255)                   |
| MEDIUMINT(size)           | A medium integer. Signed range is from -8388608 to 8388607. Unsigned range is from 0 to 16777215. The *size* parameter specifies the maximum display width (which is 255)           |
| INT(size)                 | A medium integer. Signed range is from -2147483648 to 2147483647. Unsigned range is from 0 to 4294967295. The *size* parameter specifies the maximum display width (which is 255)   |
| INTEGER(size)             | Equal to INT(size)                                                                                                                                                                  |
| BIGINT(size)              | A large integer. Signed range is from -9223372036854775808 to 9223372036854775807. Unsigned range is from 0 to 18446744073709551615. The *size* parameter specifies the maximum display width (which is 255)                                                                                                                                                                                              |
| FLOAT(size, d)            | A floating point number. The total number of digits is specified in*size* . The number of digits after the decimal point is specified in the *d* parameter. This syntax is deprecated in MySQL 8.0.17, and it will be removed in future MySQL versions                                                                                                                                                     |
| FLOAT(p)                  | A floating point number. MySQL uses the*p* value to determine whether to use FLOAT or DOUBLE for the resulting data type. If *p* is from 0 to 24, the data type becomes FLOAT(). If *p* is from 25 to 53, the data type becomes DOUBLE()                                                                                                                                                                  |
| DOUBLE(size, d)           | A normal-size floating point number. The total number of digits is specified in*size* . The number of digits after the decimal point is specified in the *d* parameter              |
| DOUBLE PRECISION(size, d) |                                                                                                                                                                                     |
| DECIMAL(size, d)          | An exact fixed-point number. The total number of digits is specified in *size* . The number of digits after the decimal point is specified in the *d* parameter. The maximum number for *size* is 65. The maximum number for *d* is 30. The default value for *size* is 10. The default value for *d* is 0.                                                                                               |
| DEC(size, d)              | Equal to DECIMAL(size,d)                                                                                                                                                            |

<!-- ! numeric data type table

| Numeric Data Type         | Description                                                                                                                                                                         |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| BIT(size)                 | A bit-value type. The number of bits per value is specified in*size* . The *size* parameter can hold a value from 1 to 64. The default value for *size* is 1.                       |
| TINYINT(size)             | A very small integer. Signed range is from -128 to 127. Unsigned range is from 0 to 255. The *size* parameter specifies the maximum display width (which is 255)                    |
| BOOL                      | Zero is considered as false, nonzero values are considered as true.                                                                                                                 |
| BOOLEAN                   | Equal to BOOL                                                                                                                                                                       |
| SMALLINT(size)            | A small integer. Signed range is from -32768 to 32767. Unsigned range is from 0 to 65535. The *size* parameter specifies the maximum display width (which is 255)                   |
| MEDIUMINT(size)           | A medium integer. Signed range is from -8388608 to 8388607. Unsigned range is from 0 to 16777215. The *size* parameter specifies the maximum display width (which is 255)           |
| INT(size)                 | A medium integer. Signed range is from -2147483648 to 2147483647. Unsigned range is from 0 to 4294967295. The *size* parameter specifies the maximum display width (which is 255)   |
| INTEGER(size)             | Equal to INT(size)                                                                                                                                                                  |
| BIGINT(size)              | A large integer. Signed range is from -9223372036854775808 to 9223372036854775807. Unsigned range is from 0 to 18446744073709551615. The *size* parameter specifies the maximum display width (which is 255)                                                                                                                                                                                              |
| FLOAT(size, d)            | A floating point number. The total number of digits is specified in*size* . The number of digits after the decimal point is specified in the *d* parameter. This syntax is deprecated in MySQL 8.0.17, and it will be removed in future MySQL versions                                                                                                                                                     |
| FLOAT(p)                  | A floating point number. MySQL uses the*p* value to determine whether to use FLOAT or DOUBLE for the resulting data type. If *p* is from 0 to 24, the data type becomes FLOAT(). If *p* is from 25 to 53, the data type becomes DOUBLE()                                                                                                                                                                  |
| DOUBLE(size, d)           | A normal-size floating point number. The total number of digits is specified in*size* . The number of digits after the decimal point is specified in the *d* parameter              |
| DOUBLE PRECISION(size, d) |                                                                                                                                                                                     |
| DECIMAL(size, d)          | An exact fixed-point number. The total number of digits is specified in *size* . The number of digits after the decimal point is specified in the *d* parameter. The maximum number for *size* is 65. The maximum number for *d* is 30. The default value for *size* is 10. The default value for *d* is 0.                                                                                               |
| DEC(size, d)              | Equal to DECIMAL(size,d)                                                                                                                                                            |

-->

> DATE/TIME DATA TYPE parameter values ([source](https://www.w3schools.com/sql/sql_datatypes.asp))

| Date/Time Data Type | Description                                                                                                                                        |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| DATE                | A date. Format: YYYY-MM-DD. The supported range is from '1000-01-01' to '9999-12-31'                                                               |
| DATETIME(fsp)       | A date and time combination. Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1000-01-01 00:00:00' to '9999-12-31 23:59:59'. Adding DEFAULT and ON UPDATE in the column definition to get automatic initialization and updating to the current date and time                                                                                                     |
| TIMESTAMP(fsp)      | A timestamp. TIMESTAMP values are stored as the number of seconds since the Unix epoch ('1970-01-01 00:00:00' UTC). Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1970-01-01 00:00:01' UTC to '2038-01-09 03:14:07' UTC. Automatic initialization and updating to the current date and time can be specified using DEFAULT CURRENT_TIMESTAMP and ON UPDATE CURRENT_TIMESTAMP in the column definition                                                                                                                                                                 |
| TIME(fsp)           | A time. Format: hh:mm:ss. The supported range is from '-838:59:59' to '838:59:59'                                                                  |
| YEAR                | A year in four-digit format. Values allowed in four-digit format: 1901 to 2155, and 0000.<br/>MySQL 8.0 does not support year in two-digit format. |

<!-- ! date/time data type table

| Date/Time Data Type | Description                                                                                                                                        |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| DATE                | A date. Format: YYYY-MM-DD. The supported range is from '1000-01-01' to '9999-12-31'                                                               |
| DATETIME(fsp)       | A date and time combination. Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1000-01-01 00:00:00' to '9999-12-31 23:59:59'. Adding DEFAULT and ON UPDATE in the column definition to get automatic initialization and updating to the current date and time                                                                                                     |
| TIMESTAMP(fsp)      | A timestamp. TIMESTAMP values are stored as the number of seconds since the Unix epoch ('1970-01-01 00:00:00' UTC). Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1970-01-01 00:00:01' UTC to '2038-01-09 03:14:07' UTC. Automatic initialization and updating to the current date and time can be specified using DEFAULT CURRENT_TIMESTAMP and ON UPDATE CURRENT_TIMESTAMP in the column definition                                                                                                                                                                 |
| TIME(fsp)           | A time. Format: hh:mm:ss. The supported range is from '-838:59:59' to '838:59:59'                                                                  |
| YEAR                | A year in four-digit format. Values allowed in four-digit format: 1901 to 2155, and 0000.<br/>MySQL 8.0 does not support year in two-digit format. |

-->

```sql
--- duplicate a table
CREATE TABLE new_table
LIKE old_table;

INSERT new_table
SELECT *
FROM old_table;
```

```sql
DROP TABLE table_name
DROP TABLE IF EXISTS table_name
```

```sql
ALTER TABLE table_name

ADD column datatype;
DROP COLUMN column
... -- rename column
... -- alter/modify datatype
... -- rename table
```

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE <condition>

-- update join
UPDATE table1 a
JOIN table2 b
  ON a.id = b.id
SET column1 = value1, column2 = value2, ...
WHERE <condition>
```

```sql
DELETE FROM table_name
WHERE <condition>
```

<!-- ? https://leetcode.com/problems/delete-duplicate-emails/solutions/3142892/explanation-of-official-solution -->

<!-- ----------------------------------------------------------------------- -->

#### INDEX

```sql
-- add index on table creation
CREATE TABLE table_name (
  ...
  INDEX idx_name (column, column)
)

-- add index after table creation
CREATE INDEX idx_name ON table_name (column1, column2, ...)
```

```sql
SHOW INDEX FROM table_name -- returns table index information
```

```sql
DROP INDEX idx_name ON table_name;
```

<!-- ----------------------------------------------------------------------- -->

### STORED FUNCTIONS AND PROCEDURES

> FUNCTION

* returns a single value

```sql
DELIMITER $$
CREATE FUNCTION function_name(function_param PARAM_DATA_TYPE)
  RETURNS <DATA_TYPE> <characteristics>
  BEGIN
    DECLARE variable_name VARIABLE_DATA_TYPE -- optional variables
    <sql_statements>
    RETURN ;
  END $$
DELIMITER ;

-- call with select statement
SELECT function_name(function_param) FROM ...
```

* `characteristics` can be
  * `NOT DETERMINISTIC` (default)
  * `DETERMINISTIC`

```sql
DROP FUNCTION function_name
```

> PROCEDURE

* group of SQL statements

```sql
CREATE PROCEDURE procedure_name
AS
<sql_statements>
GO;

EXEC procedure_name;
```

```sql
DELIMITER $$
CREATE PROCEDURE procedure_name(procedure_param PARAM_DATA_TYPE)
BEGIN
  <sql_statements>
END $$
DELIMITER ;

CALL procedure_name(procedure_param);
```

```sql
DROP PROCEDURE procedure_name
```

<!-- ----------------------------------------------------------------------- -->

### IMPORT SPREADSHEET

> MySQL Workbench

* **(Top bar)** Create a new schema in the connected server
    * Name schema > Apply > Apply > Finish
2. **(Sidebar)** SCHEMAS > new_schema > Tables
    * Right click `new_schema` > Table Data Import Wizard > File path
    * Create new table
    * Configure import settings

> Python

```python
import mysql.connector
import csv
import time

CHUNK = 10_000
CSV_FILE = './path_to/file.csv'
DB_HOST = 'localhost'
DB_USER = 'root'
DB_PASSWORD = ''
DB_NAME = 'database_name'
TABLE_NAME = 'table_name'
COLUMN_TYPES = {
  'COLUMN': 'DATA_TYPE', 
  'COLUMN': 'DATA_TYPE', 
  'COLUMN': 'DATA_TYPE' 
}

with open(CSV_FILE) as file:
  data = csv.reader(file)
  headers = next(data)
  columns = ', '.join([f'`{col}` {COLUMN_TYPES.get(col)}' for col in headers])

placeholders = ', '.join(['%s'] * len(headers))
insert_query = f'INSERT INTO {TABLE_NAME} ({', '.join(headers)}) VALUES ({placeholders})'

connection = mysql.connector.connect(
  user=DB_USER,
  password=DB_PASSWORD,
  host=DB_HOST
)

cursor = connection.cursor()

def createDatabase():
  print(f'Creating database')
  cursor.execute(f'CREATE DATABASE IF NOT EXISTS {DB_NAME}')
  cursor.execute(f'USE {DB_NAME}')

def createTable():
  print(f'Creating table')
  cursor.execute(f'DROP TABLE IF EXISTS {TABLE_NAME}')
  cursor.execute(f'CREATE TABLE {TABLE_NAME} ({columns})')

def doBulkInsert(data):
  print(f'Inserting rows')
  rows = []
  rows_inserted = 0

  for row in data:
    rows.append(tuple(row))

    if len(rows) == CHUNK:
      cursor.executemany(insert_query, rows)
      connection.commit()
      rows_inserted += len(rows)
      rows.clear()

  if rows:
    cursor.executemany(insert_query, rows)
    connection.commit()
    rows_inserted += len(rows)

  print(f'Inserted {rows_inserted} rows')

def main():
  _s = time.perf_counter()
  createDatabase()
  createTable()

  with open(CSV_FILE) as file:
    data = csv.reader(file)
    next(data)
    doBulkInsert(data)

  print(f'Duration = {time.perf_counter()-_s}')

  cursor.close()
  connection.close()

if __name__ == '__main__':
  main()
```
