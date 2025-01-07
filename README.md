# SQL Reference

1. [🔍 Query](#🔍-query) <br>
   &nbsp; • [Misc](#misc) <br>
2. [🛠️ Functions](#🛠️-functions) <br>
   &nbsp; • [📊 Aggregate Functions](#📊-aggregate-functions) <br>
   &nbsp; • [Misc Functions](#misc-functions) <br>

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
```

1. [Select](#1-select) <br>
2. [Join](#2-join) <br>
3. [Where](#3-where) <br>
4. [Group By](#4-group-by) <br>
5. [Having](#5-having) <br>
6. [Order By](#6-order-by) <br>
7. [Misc](#misc) <br>
   &nbsp; • [Clause](#clause) <br>
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
JOIN
LEFT JOIN
RIGHT JOIN
FULL JOIN
CROSS JOIN
```

`JOIN` - matching rows (default, `INNER`)<br>
`FULL JOIN` - all rows (`OUTER`) <br>
`CROSS JOIN` - match each row (cartesian product) <br>

<!-- ----------------------------------------------------------------------- -->

### 3. WHERE

* filter

```sql
WHERE NOT ...

WHERE ... AND ...
WHERE ... AND (... OR ...)

WHERE ... OR ...

WHERE ... BETWEEN ... AND ...

WHERE ... IN ...
WHERE ... IN <value1, value2, ...>
WHERE ... IN <SELECT statement>
WHERE ... NOT IN ...

WHERE ... LIKE ...
WHERE ... NOT LIKE ...

WHERE EXISTS <SELECT statement>
```
<!--
* NOT IN =/= NOT EXISTS (with NULL) https://stackoverflow.com/questions/173041/not-in-vs-not-exists
-->
<!-- ----------------------------------------------------------------------- -->

### 4. GROUP BY

<!-- ----------------------------------------------------------------------- -->

### 5. HAVING

* filter on aggregate value

<!-- ----------------------------------------------------------------------- -->

### 6. ORDER BY

```sql
ORDER BY ... ASC (default)
ORDER BY ... DESC
ORDER BY ..., ...

ORDER BY <#> (column number)
```

<!-- ----------------------------------------------------------------------- -->

### MISC

1. [Clause](#clause) <br>
2. [Operator](#operator) <br>
3. [Expression](#expression) <br>
4. [Wildcards](#wildcards) <br>

<!-- ----------------------------------------------------------------------- -->

#### CLAUSE

```sql
LIMIT <#>
```

<!-- ----------------------------------------------------------------------- -->

#### OPERATOR

```sql
IS NULL
IS NOT NULL
```

see [`ISNULL()`]()

<!-- ----------------------------------------------------------------------- -->

#### EXPRESSION

> CASE

* basically if/then logic
* see function [`IF()`]()

```sql
CASE
   WHEN <condition> THEN <result>
      ...
   WHEN <condition> THEN <result>
   ELSE <result>
END AS '<alias>'
```

```sql
CASE col
   WHEN 'value' THEN 'result'
      ...
   WHEN 'value' THEN 'result'
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
```

> _ - single character

```sql
'_anyfollowedby'
```

<!-- ----------------------------------------------------------------------- -->

## 🛠️ FUNCTIONS

1. [📊 Aggregate Functions](#📊-aggregate-functions) <br>
2. [Misc Functions](#2-join) <br>
   &nbsp; • [Math](#math) <br>
   &nbsp; • [String](#string) <br>
   &nbsp; • [📆 Date](#📆-date) <br>

<!-- ----------------------------------------------------------------------- -->

### 📊 AGGREGATE FUNCTIONS

```sql
SUM()
```

`SUM(<condition>)` - the count of rows where the condition is TRUE <br>

`SUM(CASE WHEN <condition> THEN col ELSE 0 END)` - sum of condition <!-- (see [`CASE`](#case)) --> (preferred)<br>
`SUM((<condition>) * col)` - sum of condition <!-- ! --> (if condition is `NULL`, the row is ignored)

```sql
AVG()
```

```sql
MIN()
```

```sql
MAX()
```

```sql
COUNT()
```

`COUNT(*)` - all rows (NULL rows counted) <br>
`COUNT(col)` - non-NULL rows (NULL rows <ins>**not**</ins> counted) <br>

| EXPRESSION   | NOTES         | NULL rows counted |
| ------------ | ------------- | ----------------- |
| `COUNT(*)`   | all rows      | yes               |
| `COUNT(col)` | non-NULL rows | no                |

<!-- ----------------------------------------------------------------------- -->

### MISC FUNCTIONS

#### CONDITIONAL BRANCHING

```sql
IF(condition, value_if_true, value_if_false)
```
<!--
* `value_if_true` and `value_if_false` are <ins>**required**</ins>

> SQL Server

```sql
IIF(condition, value_if_true, value_if_false)
```

* `value_if_true` and `value_if_false` are <ins>**optional**</ins>
-->

#### MATH

```sql
ROUND(number, decimal_value, )
```

#### STRING
<!-- 🔤🔠💬 -->

```sql
LENGTH(string)
```

#### 📆 DATE

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
