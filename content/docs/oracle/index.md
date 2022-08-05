title: "Oracle"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

# Define a variable in Oracle
``` sql
DEFINE formatt = 'FM9999999999999999999990.00'

select
    to_char(10000, '&formatt') as amount
from DUAL
```