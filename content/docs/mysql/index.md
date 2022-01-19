---
title: "Mysql"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
## Creating a schema and giving an user network access
``` SQL

create schema mySchema; 

CREATE USER 'myUser@'%' IDENTIFIED BY 'myUser';

-- The % bellow is the wildcard to accept connection from the client of any host in the network
grant all privileges on mySchema.* to 'myUser'@'%' identified by 'myUser'; 
```