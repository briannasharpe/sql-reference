# SQL Reference

1. [Query](#query) <br>
   &nbsp; • [Misc](#misc) <br>
2. [Functions](#functions) <br>
   &nbsp; • [Aggregate Functions](#aggregate-functions) <br>
   &nbsp; • [Misc Functions](#misc-functions) <br>

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
```

1. [Select](#1-select) <br>
2. [Join](#2-join) <br>
3. [Where](#3-where) <br>
4. [Group By](#4-group-by) <br>
5. [Having](#5-having) <br>
6. [Order By](#6-order-by) <br>
7. [Misc](#misc) <br>
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

> filter

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

<!-- ----------------------------------------------------------------------- -->

### 4. GROUP BY

<!-- ----------------------------------------------------------------------- -->

### 5. HAVING

> filter on aggregate value

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

```sql
IF(condition, value_if_true, value_if_false)
```

```sql
IS NULL
IS NOT NULL
```

```sql
LIMIT <#>
```

<!-- ----------------------------------------------------------------------- -->

### WILDCARDS

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

## FUNCTIONS

<!-- ----------------------------------------------------------------------- -->

### AGGREGATE FUNCTIONS

```sql
SUM()
```

`SUM(<condition>)` - the count of rows where the condition is TRUE <br>

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

```sql
ROUND(number, decimal_value, )
```

```sql
LENGTH(string)
```

```sql
DATEDIFF(date1, date2)
```
