---
title: "MySQL"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---

## Enabling MySQL remote access

Just run the mysql server with the parameter
```sh
--bind-address=0.0.0.0
```
Example when in a docker compose file:

``` yaml
...
  mysql-dev:
    image: mysql:8.0.4
    command:
     - --bind-address=0.0.0.0
    environment:
      MYSQL_DATABASE: mysql
      MYSQL_PASSWORD: mysql
      MYSQL_ROOT_PASSWORD: mysql
      MYSQL_USER: mysql
    ports:
     - 3306:3306
    volumes:
     - mysql-data:/var/lib/mysql
```
Or, you can add the `bind-address=0.0.0.0` in the `/etc/mysql/my.cnf` file and restart the server.


## Creating a schema and giving an user network access in Mysql 5 and 8

``` SQL

create schema mySchema; 

CREATE USER 'myUser'@'%' IDENTIFIED BY 'myUser';

-- The % bellow is the wildcard to accept connection from the client of any host in the network
grant all privileges on mySchema.* to 'myUser'@'%' identified by 'myUser'; 
```

Then, the permission granting in Mysql 5
``` SQL
-- The % bellow is the wildcard to accept connection from the client of any host in the network
grant all privileges on mySchema.* to 'myUser'@'%' identified by 'myUser'; 
```

Or, the permission granting in Mysql 8
``` SQL
-- The % bellow is the wildcard to accept connection from the client of any host in the network
grant all privileges on mySchema.* to 'myUser'@'%''; 
```

## Backing up the mysql database
Just one table:
``` bash
mysqldump -u user -ppassword -h 127.0.0.1 -P 3306 --skip-mysql-schema my_db MY_TABLE > dump.sql
```

Just the schema and ignoring one table:
``` bash
mysqldump -u user -ppassword  -h 127.0.0.1 -P 3306 --ignore-table my_db.table1 --no-data my_db > nodata.sql
```

Skipping the schema and ignoring two tables:
``` bash
 mysqldump -u user -ppassword  -h 127.0.0.1 -P 3306 --skip-mysql-schema --ignore-table my_db.table1 --ignore-table my_db.table2 my_db > smallertables.sql
```

## Execute a script
Also when you want to import the mysqldump file.

``` bash
mysql -u user -ppassword my_database -h 127.0.0.1 -P 3606 < dump.sql
```

## Maintenance

### Delete logs to increase space

MySQL keeps some logs for doing its business, to delete it you do:
``` mysql
RESET MASTER; 
--or, if the instance is a slave
RESET SLAVE;
```

### Optimize the space shrinking tables

The MySQL does not create space when you delete data. For claiming up the space, you need to do:
``` mysql
OPTIMIZE TABLE MY_TABLE;
```
For doing it you have to have some space in the machine. Internally, it will create a new file more optimized for the table, and then delete the old one.
