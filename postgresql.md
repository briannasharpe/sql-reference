# PostgreSQL Reference

* [MySQL](README.md)
* **PostgreSQL** (current)

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
  • [Export Table](#export-table)

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

<!-- ----------------------------------------------------------------------- -->

### 2. JOIN

<!-- ----------------------------------------------------------------------- -->

### 3. WHERE

<!-- ----------------------------------------------------------------------- -->

```sql
-- regular expressions
WHERE ... ~ '<pattern>' -- case sensitive
WHERE ... ~* '<pattern>' -- case insensitive
WHERE ... !~ '<pattern>' -- case sensitive (unmatched)
WHERE ... !~* '<pattern>' -- case insensitive (unmatched)

-- example
WHERE mail ~ '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode[.]com$'
```

<!-- ----------------------------------------------------------------------- -->

### 4. GROUP BY

<!-- ----------------------------------------------------------------------- -->

### 5. HAVING

<!-- ----------------------------------------------------------------------- -->

### 6. ORDER BY

<!-- ----------------------------------------------------------------------- -->

### 7. LIMIT

<!-- ----------------------------------------------------------------------- -->

### MISC

1. [Common Table Expression (CTE)](#common-table-expression)  
2. [Operator](#operator)  
3. [Expression](#expression)  
4. [Wildcards](#wildcards)  
5. [Commands](#commands)  

<!-- ----------------------------------------------------------------------- -->

#### COMMON TABLE EXPRESSION

<!-- ----------------------------------------------------------------------- -->

#### OPERATOR

<!-- ----------------------------------------------------------------------- -->

#### EXPRESSION

<!-- ----------------------------------------------------------------------- -->

#### WILDCARDS

<!-- ----------------------------------------------------------------------- -->

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

<!-- ----------------------------------------------------------------------- -->

### WINDOW FUNCTIONS

<!-- ----------------------------------------------------------------------- -->

### MISC FUNCTIONS

<!-- ----------------------------------------------------------------------- -->

#### NULL

<!-- ----------------------------------------------------------------------- -->

#### CONDITIONAL BRANCHING

<!-- ----------------------------------------------------------------------- -->

#### MATH

```sql
ROUND(number, decimals) -- decimals is optional, rounds to nearest integer if not specified
```

* `number` - value to be rounded
* `decimals` - decimal places to round

<!-- ----------------------------------------------------------------------- -->

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
REGEXP_REPLACE(expression, pattern, replacement, flags) -- flags is optional
```

* flags
  * `'g'` - replace all occurences
    * without `'g'`, only first match is replaced
  * `'i'` - case insensitive matching
  * `'n'` - matching any character and newline characters

<!-- ----------------------------------------------------------------------- -->

#### DATE

<!-- ----------------------------------------------------------------------- -->

## DATABASE

1. [General](#general)  
  • [Index](#index)  
2. [Stored Functions and Procedures](#stored-functions-and-procedures)  
3. [Import Spreadsheet](#import-spreadsheet)  
4. [Export Table](#export-table)  

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

<!-- ----------------------------------------------------------------------- -->

#### INDEX

<!-- ----------------------------------------------------------------------- -->

### STORED FUNCTIONS AND PROCEDURES

<!-- ----------------------------------------------------------------------- -->

### IMPORT SPREADSHEET

> pgAdmin

1. Create a new server
    * Right click 'Servers' > Register > Server
      * General > Name server
      * Connection > Host name/address & Password
2. Create a new database
    * Right click 'Databases' > Create
      * General > Name database
3. Create a new schema
    * Right click 'Schemas' > Create
      * General > Name schema
4. Create a new table
    * Right click 'Schemas' > Query Tool
    * Run `CREATE TABLE table_name (...)` with specified values
    * Right click `table_name` > Import/Export data

<!-- ----------------------------------------------------------------------- -->

### EXPORT TABLE

> pgAdmin

* Right click `table_name` table > Import/Export data

> Python

```python
import pandas as pd
from sqlalchemy import create_engine
import time
import os

DB_PORT = '5432'
DB_HOST = 'localhost'
DB_USER = 'postgres'
DB_PASSWORD = ''
DB_NAME = 'database_name'
TABLES = ['table_name', 'table_name', ...]
OUTPUT_FILE = 'file_name.xlsx'
OUTPUT_FOLDER = rf'.\folder'
OUTPUT_PATH = os.path.join(OUTPUT_FOLDER, OUTPUT_FILE)

engine = create_engine(f'postgresql+psycopg2://{DB_USER}:{DB_PASSWORD}@{DB_HOST}:{DB_PORT}/{DB_NAME}')
writer = pd.ExcelWriter(OUTPUT_PATH, engine='xlsxwriter')
_s = time.perf_counter()

for table in TABLES:
  print(f'Fetching table data for {table}')
  query = f'SELECT * FROM {DB_NAME}.{table}'

  print(f'Writing data to excel sheet')
  df = pd.read_sql(query, engine)
  df.to_excel(writer, sheet_name=table, index=False)

writer.close()
print(f'Exported to {OUTPUT_PATH}')
print(f'Duration = {time.perf_counter()-_s}') 

```
