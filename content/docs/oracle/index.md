---
title: "Oracle"
weight: 1
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

When you have dynamic, let's say 3, number of decimals:

``` sql
select
    TO_CHAR(10000 / power(10, 3),(('999999999999990.'|| LTRIM((Power(10, 3)), 1)))) as net_amount,
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