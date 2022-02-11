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
      MYSQL_DATABASE: easypnr_dev
      MYSQL_PASSWORD: easypnr_dev
      MYSQL_ROOT_PASSWORD: aoeu1212
      MYSQL_USER: easypnr_dev
    ports:
     - 3306:3306
    volumes:
     - mysql-data-easypnr-dev:/var/lib/mysql
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
``` ssh
mysqldump -u user -ppassword -h 127.0.0.1 -P 3306 --skip-mysql-schema my_database MY_TABLE > dump.sql
```

## Execute a script
Also when you want to import the mysqldump file.

``` ssh
mysql -u user -ppassword my_database -h 127.0.0.1 -P 3606 < dump.sql
```