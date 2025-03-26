# PostgreSQL Reference

* [MySQL](README.md)
* **PostgreSQL** (current)

1. [📁 Query](#query) <br>
  <!-- • [📂 Misc](#misc) <br>
  &nbsp; ∘ [Common Table Expression (CTE)](#common-table-expression) <br>
  &nbsp; ∘ [Operator](#operator) <br>
  &nbsp; ∘ [Expression](#expression) <br>
  &nbsp; ∘ [Wildcards](#wildcards) <br>
  &nbsp; ∘ [Commands](#commands) <br> -->
2. [🛠️ Functions](#functions) <br>
  <!-- • [📊 Aggregate Functions](#aggregate-functions) <br>
  • [🔍 Window Functions](#window-functions) <br>
  • [🛠️ Misc Functions](#misc-functions) <br>
  &nbsp; ∘ [⛔ Null](#null) <br>
  &nbsp; ∘ [❔ Conditional Branching](#conditional-branching) <br>
  &nbsp; ∘ [📐 Math](#math) <br>
  &nbsp; ∘ [💬 String](#string) <br>
  &nbsp; ∘ [📆 Date](#date) <br> -->
3. [🗃️ Database](#database) <br>
  • [General](#general) <br>
  <!-- • [Stored Functions and Procedures](#stored-functions-and-procedures) <br>
  • [Import Spreadsheet](#import-spreadsheet) -->
<!-- 4. [pgAdmin](#pgadmin) -->

<!-- ----------------------------------------------------------------------- -->

## QUERY

1. [Select](#1-select) <br>
2. [Join](#2-join) <br>
3. [Where](#3-where) <br>
4. [Group By](#4-group-by) <br>
5. [Having](#5-having) <br>
6. [Order By](#6-order-by) <br>
7. [Limit](#7-limit)
8. [Misc](#misc) <br>
  <!-- • [Common Table Expression (CTE)](#common-table-expression) <br>
  • [Operator](#operator) <br>
  • [Expression](#expression) <br>
  • [Wildcards](#wildcards) -->

<!-- ----------------------------------------------------------------------- -->

### 1. SELECT

### 2. JOIN

### 3. WHERE

```sql
-- regular expressions
WHERE ... ~ '<pattern>' -- case sensitive
WHERE ... ~* '<pattern>' -- case insensitive
WHERE ... !~ '<pattern>' -- case sensitive (unmatched)
WHERE ... !~* '<pattern>' -- case insensitive (unmatched)

-- example
WHERE mail ~ '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode[.]com$'
```

### 4. GROUP BY

### 5. HAVING

### 6. ORDER BY

### 7. LIMIT

### MISC

<!-- 1. [Common Table Expression (CTE)](#common-table-expression)
2. [Operator](#operator) <br>
3. [Expression](#expression) <br>
4. [Wildcards](#wildcards) <br>
5. [Commands](#commands) <br> -->

#### COMMON TABLE EXPRESSION

#### OPERATOR

#### EXPRESSION

#### WILDCARDS

#### COMMANDS

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

### WINDOW FUNCTIONS

### MISC FUNCTIONS

#### NULL

#### CONDITIONAL BRANCHING

#### MATH

#### STRING

```sql
'strings' || 'to' || 'concatenate' -- 'strings to concatenate'
```

* add strings together

```sql
SPLIT_PART(string, delimiter, number)
```

* `delimiter` - the mark to search for
* `number` - number of times to search delimiter
  * positive number = all left of delimiter
  * negative number = all right of delimiter

```sql
REGEXP_REPLACE(expression, pattern, replacement)

--optional: flags
REGEXP_REPLACE(expression, pattern, replacement, flags)
```

* flags
  * `'g'` - replace all occurences 
    * without `'g'`, only first match is replaced
  * `'i'` - case insensitive matching
  * `'n'` - matching any character and newline characters

#### DATE

<!-- ----------------------------------------------------------------------- -->

## DATABASE

1. [General](#general) <br>
  <!-- • [Index](#index) <br> -->
<!-- 2. [Stored Functions and Procedures](#stored-functions-and-procedures) <br>
3. [Import Spreadsheet](#import-spreadsheet) -->

<!-- ----------------------------------------------------------------------- -->

### GENERAL

```sql
SHOW search_path

SET search_path TO schema_name -- temporary
ALTER ROLE username SET search_path = schema_name -- persistent
ALTER DATABASE database_name SET search_path = schema_name -- persistent

SET search_path TO DEFAULT -- reset
```

* to change query from `SELECT * FROM schema.table` to `SELECT * FROM table`

```sql
-- duplicate a table
CREATE TABLE new_table AS
SELECT *
FROM old_table;
```

```sql
-- update join
UPDATE table1
SET column1 = value1, column2 = value2, ...
FROM table2
WHERE <condition> -- table1.column = table2.column, ...
```

### STORED FUNCTIONS AND PROCEDURES

### IMPORT SPREADSHEET
