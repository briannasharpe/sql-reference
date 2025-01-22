# SQL Reference

1. [🔍 Query](#🔍-query) <br>
   &nbsp; • [Misc](#misc) <br>
2. [🛠️ Functions](#🛠️-functions) <br>
   &nbsp; • [📊 Aggregate Functions](#📊-aggregate-functions) <br>
   &nbsp; • [Window Functions](#window-functions) <br>
   &nbsp; • [Misc Functions](#misc-functions) <br>
3. [🗃️ Database](#🗃️-database)

<!-- ----------------------------------------------------------------------- -->

## 🔍 QUERY

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

1. [Select](#1-select) <br>
2. [Join](#2-join) <br>
3. [Where](#3-where) <br>
4. [Group By](#4-group-by) <br>
5. [Having](#5-having) <br>
6. [Order By](#6-order-by) <br>
7. [Limit](#7-limit)
8. [Misc](#misc) <br>
   &nbsp; • [Operator](#operator) <br>
   &nbsp; • [Expression](#expression) <br>
   &nbsp; • [Wildcards](#wildcards)

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

* `JOIN` - matching rows <br>
* `FULL JOIN` - all rows <br>
* `CROSS JOIN` - match each row (cartesian product) <br>

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
```

* see [WILDCARDS](#wildcards)

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
2. [Operator](#operator) <br>
3. [Expression](#expression) <br>
4. [Wildcards](#wildcards) <br>

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

## 🛠️ FUNCTIONS

1. [📊 Aggregate Functions](#📊-aggregate-functions) <br>
2. [Window Functions](#window-functions) <br>
3. [Misc Functions](#2-join) <br>
   &nbsp; • [Null](#null) <br>
   &nbsp; • [Conditional Branching](#conditional-branching) <br>
   &nbsp; • [Math](#math) <br>
   &nbsp; • [String](#string) <br>
   &nbsp; • [📆 Date](#📆-date) <br>

<!-- ----------------------------------------------------------------------- -->

### 📊 AGGREGATE FUNCTIONS

```sql
SUM()
```

* `SUM(<condition>)` - the count of rows where the condition is TRUE <br>
* `SUM(CASE WHEN <condition> THEN col ELSE 0 END)` - sum of condition <!-- (see [`CASE`](#case)) --> (preferred)<br>
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

* `COUNT(*)` - all rows (`NULL` rows counted) <br>
* `COUNT(col)` - non-NULL rows (`NULL` rows <ins>**not**</ins> counted) <br>

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

* see [`DATE_ADD` and `DATE_SUB` parameter values](#📆-date)

<!-- ----------------------------------------------------------------------- -->

### MISC FUNCTIONS

#### NULL

```sql
ISNULL(expression)
```

```sql
IFNULL(expression, alt_value)
```

* `expression` - expression to test
* `alt_value` - return value if expression is NULL

#### CONDITIONAL BRANCHING

```sql
IF(condition, value_if_true, value_if_false)
```

#### MATH

```sql
ROUND(number, decimal_value, )
```

```sql
MOD(x, y) -- %
```

#### STRING

<!-- 🔤🔠💬 -->

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

#### 📆 DATE

```sql
DAY()
MONTH()
YEAR()
```

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
| unit      | Required. The type of interval to add/subtract. Can be one of the following values:<li>MICROSECOND</li> <li>SECOND</li> <li>MINUTE</li> <li>HOUR</li> <li>DAY</li> <li>WEEK</li> <li>MONTH</li> <li>QUARTER</li> <li>YEAR</li> <li>SECOND_MICROSECOND</li> <li>MINUTE_MICROSECOND</li> <li>MINUTE_SECOND</li> <li>HOUR_MICROSECOND</li> <li>HOUR_SECOND</li> <li>HOUR_MINUTE</li> <li>DAY_MICROSECOND</li> <li>DAY_SECOND</li> <li>DAY_MINUTE</li> <li>DAY_HOUR</li> <li>YEAR_MONTH<li>                                                                                                                     |

<!-- ! DATE_ADD / DATE_SUB table

| Parameter | Description                                                                                                              |
| --------- | ------------------------------------------------------------------------------------------------------------------------ |
| date      | Required. The date to be modified                                                                                        |
| value     | Required. The value of the time/date interval to add/subtract. Both positive and negative values are allowed             |
| unit      | Required. The type of interval to add/subtract. Can be one of the following values:<li>MICROSECOND</li> <li>SECOND</li> <li>MINUTE</li> <li>HOUR</li> <li>DAY</li> <li>WEEK</li> <li>MONTH</li> <li>QUARTER</li> <li>YEAR</li> <li>SECOND_MICROSECOND</li> <li>MINUTE_MICROSECOND</li> <li>MINUTE_SECOND</li> <li>HOUR_MICROSECOND</li> <li>HOUR_SECOND</li> <li>HOUR_MINUTE</li> <li>DAY_MICROSECOND</li> <li>DAY_SECOND</li> <li>DAY_MINUTE</li> <li>DAY_HOUR</li> <li>YEAR_MONTH<li>                                                                                                                     |

-->

<!-- ----------------------------------------------------------------------- -->

## 🗃️ DATABASE

```sql
CREATE DATABASE database_name
DROP DATABASE database_name
```

```sql
CREATE TABLE table_name (
   column1 datatype,
   ...
   column# datatype
)
```

> data type parameter values ([source](https://www.w3schools.com/sql/sql_datatypes.asp))

| String data type            | Description                                                                                                                            |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| CHAR(size)                  | A FIXED length string (can contain letters, numbers, and special characters). The*size* parameter specifies the column length in characters - can be from 0 to 255. Default is 1                                                                                                                                            |
| VARCHAR(size)               | A VARIABLE length string (can contain letters, numbers, and special characters). The*size* parameter specifies the maximum string length in characters - can be from 0 to 65535                                                                                                                                                 |
| BINARY(size)                | Equal to CHAR(), but stores binary byte strings. The*size* parameter specifies the column length in bytes. Default is 1                |
| VARBINARY(size)             | Equal to VARCHAR(), but stores binary byte strings. The*size* parameter specifies the maximum column length in                         |
| TINYBLOB                    | For BLOBs (Binary Large Objects). Max length: 255 bytes                                                                                |
| TINYTEXT                    | Holds a string with a maximum length of 255 characters                                                                                 |
| TEXT(size)                  | Holds a string with a maximum length of 65,535 bytes                                                                                   |
| BLOB(size)                  | For BLOBs (Binary Large Objects). Holds up to 65,535 bytes of data                                                                     |
| MEDIUMTEXT                  | Holds a string with a maximum length of 16,777,215 characters                                                                          |
| MEDIUMBLOB                  | For BLOBs (Binary Large Objects). Holds up to 16,777,215 bytes of data                                                                 |
| LONGTEXT                    | Holds a string with a maximum length of 4,294,967,295 characters                                                                       |
| LONGBLOB                    | For BLOBs (Binary Large Objects). Holds up to 4,294,967,295 bytes of data                                                              |
| ENUM(val1, val2, val3, ...) | A string object that can have only one value, chosen from a list of possible values. You can list up to 65535 values in an ENUM list. If a value is inserted that is not in the list, a blank value will be inserted. The values are sorted in the order you enter them                                                    |
| SET(val1, val2, val3, ...)  | A string object that can have 0 or more values, chosen from a list of possible values. You can list up to 64 values in a SET list      |

<!-- ! string data type table

| String data type            | Description                                                                                                                            |
| --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| CHAR(size)                  | A FIXED length string (can contain letters, numbers, and special characters). The*size* parameter specifies the column length in characters - can be from 0 to 255. Default is 1                                                                                                                                            |
| VARCHAR(size)               | A VARIABLE length string (can contain letters, numbers, and special characters). The*size* parameter specifies the maximum string length in characters - can be from 0 to 65535                                                                                                                                                 |
| BINARY(size)                | Equal to CHAR(), but stores binary byte strings. The*size* parameter specifies the column length in bytes. Default is 1                |
| VARBINARY(size)             | Equal to VARCHAR(), but stores binary byte strings. The*size* parameter specifies the maximum column length in                         |
| TINYBLOB                    | For BLOBs (Binary Large Objects). Max length: 255 bytes                                                                                |
| TINYTEXT                    | Holds a string with a maximum length of 255 characters                                                                                 |
| TEXT(size)                  | Holds a string with a maximum length of 65,535 bytes                                                                                   |
| BLOB(size)                  | For BLOBs (Binary Large Objects). Holds up to 65,535 bytes of data                                                                     |
| MEDIUMTEXT                  | Holds a string with a maximum length of 16,777,215 characters                                                                          |
| MEDIUMBLOB                  | For BLOBs (Binary Large Objects). Holds up to 16,777,215 bytes of data                                                                 |
| LONGTEXT                    | Holds a string with a maximum length of 4,294,967,295 characters                                                                       |
| LONGBLOB                    | For BLOBs (Binary Large Objects). Holds up to 4,294,967,295 bytes of data                                                              |
| ENUM(val1, val2, val3, ...) | A string object that can have only one value, chosen from a list of possible values. You can list up to 65535 values in an ENUM list. If a value is inserted that is not in the list, a blank value will be inserted. The values are sorted in the order you enter them                                                    |
| SET(val1, val2, val3, ...)  | A string object that can have 0 or more values, chosen from a list of possible values. You can list up to 64 values in a SET list      |

-->

| Numeric Data Type               | Description                                                                                                                            |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| BIT(*size* )                    | A bit-value type. The number of bits per value is specified in*size* . The *size* parameter can hold a value from 1 to 64. The default value for *size* is 1.                                                                                                                                                               |
| TINYINT(*size* )                | A very small integer. Signed range is from -128 to 127. Unsigned range is from 0 to 255. The*size* parameter specifies the maximum display width (which is 255)                                                                                                                                                             |
| BOOL                            | Zero is considered as false, nonzero values are considered as true.                                                                    |
| BOOLEAN                         | Equal to BOOL                                                                                                                          |
| SMALLINT(*size* )               | A small integer. Signed range is from -32768 to 32767. Unsigned range is from 0 to 65535. The*size* parameter specifies the maximum display width (which is 255)                                                                                                                                                             |
| MEDIUMINT(*size* )              | A medium integer. Signed range is from -8388608 to 8388607. Unsigned range is from 0 to 16777215. The*size* parameter specifies the maximum display width (which is 255)                                                                                                                                                       |
| INT(*size* )                    | A medium integer. Signed range is from -2147483648 to 2147483647. Unsigned range is from 0 to 4294967295. The*size* parameter specifies the maximum display width (which is 255)                                                                                                                                               |
| INTEGER(*size* )                | Equal to INT(size)                                                                                                                     |
| BIGINT(*size* )                 | A large integer. Signed range is from -9223372036854775808 to 9223372036854775807. Unsigned range is from 0 to 18446744073709551615. The*size* parameter specifies the maximum display width (which is 255)                                                                                                               |
| FLOAT(*size* , *d* )            | A floating point number. The total number of digits is specified in*size* . The number of digits after the decimal point is specified in the *d* parameter. This syntax is deprecated in MySQL 8.0.17, and it will be removed in future MySQL versions                                                                      |
| FLOAT(*p* )                     | A floating point number. MySQL uses the*p* value to determine whether to use FLOAT or DOUBLE for the resulting data type. If *p* is from 0 to 24, the data type becomes FLOAT(). If *p* is from 25 to 53, the data type becomes DOUBLE()                                                                                     |
| DOUBLE(*size* , *d* )           | A normal-size floating point number. The total number of digits is specified in*size* . The number of digits after the decimal point is specified in the *d* parameter                                                                                                                                                          |
| DOUBLE PRECISION(*size* , *d* ) |                                                                                                                                        |
| DECIMAL(*size* , *d* )          | An exact fixed-point number. The total number of digits is specified in*size* . The number of digits after the decimal point is specified in the *d* parameter. The maximum number for *size* is 65. The maximum number for *d* is 30. The default value for *size* is 10. The default value for *d* is 0.                      |
| DEC(*size* , *d* )              | Equal to DECIMAL(size,d)                                                                                                               |

<!-- ! numeric data type table

| Numeric Data Type               | Description                                                                                                                            |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| BIT(*size* )                    | A bit-value type. The number of bits per value is specified in*size* . The *size* parameter can hold a value from 1 to 64. The default value for *size* is 1.                                                                                                                                                               |
| TINYINT(*size* )                | A very small integer. Signed range is from -128 to 127. Unsigned range is from 0 to 255. The*size* parameter specifies the maximum display width (which is 255)                                                                                                                                                             |
| BOOL                            | Zero is considered as false, nonzero values are considered as true.                                                                    |
| BOOLEAN                         | Equal to BOOL                                                                                                                          |
| SMALLINT(*size* )               | A small integer. Signed range is from -32768 to 32767. Unsigned range is from 0 to 65535. The*size* parameter specifies the maximum display width (which is 255)                                                                                                                                                             |
| MEDIUMINT(*size* )              | A medium integer. Signed range is from -8388608 to 8388607. Unsigned range is from 0 to 16777215. The*size* parameter specifies the maximum display width (which is 255)                                                                                                                                                       |
| INT(*size* )                    | A medium integer. Signed range is from -2147483648 to 2147483647. Unsigned range is from 0 to 4294967295. The*size* parameter specifies the maximum display width (which is 255)                                                                                                                                               |
| INTEGER(*size* )                | Equal to INT(size)                                                                                                                     |
| BIGINT(*size* )                 | A large integer. Signed range is from -9223372036854775808 to 9223372036854775807. Unsigned range is from 0 to 18446744073709551615. The*size* parameter specifies the maximum display width (which is 255)                                                                                                               |
| FLOAT(*size* , *d* )            | A floating point number. The total number of digits is specified in*size* . The number of digits after the decimal point is specified in the *d* parameter. This syntax is deprecated in MySQL 8.0.17, and it will be removed in future MySQL versions                                                                      |
| FLOAT(*p* )                     | A floating point number. MySQL uses the*p* value to determine whether to use FLOAT or DOUBLE for the resulting data type. If *p* is from 0 to 24, the data type becomes FLOAT(). If *p* is from 25 to 53, the data type becomes DOUBLE()                                                                                     |
| DOUBLE(*size* , *d* )           | A normal-size floating point number. The total number of digits is specified in*size* . The number of digits after the decimal point is specified in the *d* parameter                                                                                                                                                          |
| DOUBLE PRECISION(*size* , *d* ) |                                                                                                                                        |
| DECIMAL(*size* , *d* )          | An exact fixed-point number. The total number of digits is specified in*size* . The number of digits after the decimal point is specified in the *d* parameter. The maximum number for *size* is 65. The maximum number for *d* is 30. The default value for *size* is 10. The default value for *d* is 0.                      |
| DEC(*size* , *d* )              | Equal to DECIMAL(size,d)                                                                                                               |

-->

| Date/Time Data Type | Description                                                                                                                                        |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| DATE                | A date. Format: YYYY-MM-DD. The supported range is from '1000-01-01' to '9999-12-31'                                                               |
| DATETIME(*fsp* )    | A date and time combination. Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1000-01-01 00:00:00' to '9999-12-31 23:59:59'. Adding DEFAULT and ON UPDATE in the column definition to get automatic initialization and updating to the current date and time                                                                  |
| TIMESTAMP(*fsp* )   | A timestamp. TIMESTAMP values are stored as the number of seconds since the Unix epoch ('1970-01-01 00:00:00' UTC). Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1970-01-01 00:00:01' UTC to '2038-01-09 03:14:07' UTC. Automatic initialization and updating to the current date and time can be specified using DEFAULT CURRENT_TIMESTAMP and ON UPDATE CURRENT_TIMESTAMP in the column definition                                                                                                 |
| TIME(*fsp* )        | A time. Format: hh:mm:ss. The supported range is from '-838:59:59' to '838:59:59'                                                                  |
| YEAR                | A year in four-digit format. Values allowed in four-digit format: 1901 to 2155, and 0000.<br/>MySQL 8.0 does not support year in two-digit format. |

<!-- ! date/time data type table

| Date/Time Data Type | Description                                                                                                                                        |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| DATE                | A date. Format: YYYY-MM-DD. The supported range is from '1000-01-01' to '9999-12-31'                                                               |
| DATETIME(*fsp* )    | A date and time combination. Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1000-01-01 00:00:00' to '9999-12-31 23:59:59'. Adding DEFAULT and ON UPDATE in the column definition to get automatic initialization and updating to the current date and time                                                                  |
| TIMESTAMP(*fsp* )   | A timestamp. TIMESTAMP values are stored as the number of seconds since the Unix epoch ('1970-01-01 00:00:00' UTC). Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1970-01-01 00:00:01' UTC to '2038-01-09 03:14:07' UTC. Automatic initialization and updating to the current date and time can be specified using DEFAULT CURRENT_TIMESTAMP and ON UPDATE CURRENT_TIMESTAMP in the column definition                                                                                                 |
| TIME(*fsp* )        | A time. Format: hh:mm:ss. The supported range is from '-838:59:59' to '838:59:59'                                                                  |
| YEAR                | A year in four-digit format. Values allowed in four-digit format: 1901 to 2155, and 0000.<br/>MySQL 8.0 does not support year in two-digit format. |

-->

```sql
DROP TABLE table_name
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
DELETE FROM table_name
WHERE <condition>
```

<!-- ? https://leetcode.com/problems/delete-duplicate-emails/solutions/3142892/explanation-of-official-solution >
