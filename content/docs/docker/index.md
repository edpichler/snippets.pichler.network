---
title: "Docker"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Docker is awesome
Because of many reasons. But there is something in containerization and virtualization that attracts me very much. I think it's the ultimate separation of concerns, the break down of complexity and declouping of system components. 

## List all containers:
 ```bash
 docker ps -a
 ```

## Delete forcevily all the containers:
 ```bash
 docker rm -f $(docker ps -a -q)
 ```
