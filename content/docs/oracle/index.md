---
title: "Oracle"
weight: 10
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

## Define a variable in Oracle
``` sql
DEFINE formatt = 'FM9999999999999999999990.00'

select
    to_char(10000, '&formatt') as amount
from DUAL;
```

### When you have a dynamic, let's say 3, number of decimals:

``` sql
select
    TO_CHAR(10000 / power(10, 3),(('999999999999990.'|| LTRIM((Power(10, 3)), 1)))) as my_amount,
from DUAL;
```

## Format output in Oracle when you have very long columns 

``` bash
SQL>  SET linesize 5000
SQL>  SET tab off
SQL>  select * from table_with_many_long_columns;
```

Another possible way of doing it:

``` bash
SET WRAP OFF
SET PAGESIZE 0
```

## Select from a list of static data

This must work in any ANSI compatible database:

``` sql
WITH my_table as (
    select 'one', 'a' from dual
    union all
    select 'two', 'b' from dual
)
select * from my_table;
```

## Drop table if not exists, in Oracle

Oracle, depending of the version, may not have the DROP IF EXISTS command, so you do it in an "Oracle Anonymous Procedure":

``` sql
BEGIN
    execute immediate 'DROP TABLE aiai';
EXCEPTION
    WHEN OTHERS THEN
        IF SQLCODE != -942 THEN
            RAISE;
        END IF;
END;

```

