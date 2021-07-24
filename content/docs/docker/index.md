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
There is something in containerization and virtualization that attracts me very much. I think it's the ultimate separation of concerns, the break down of complexity and declouping of system components. 

## Running a container exposing a port:
Exposes the container 80 port as a local/host 8080:
 ```bash
 docker run -p 8080:80 edpichler/nginx
 ```

 ## Entering inside a running docker container

 ``` bash
    docker exec -it e4383e55958d /bin/bash
 ```

## Listeing all containers:
 ```bash
 docker ps -a
 ``` 

## Deleting forcevily all the containers:
 ```bash
 docker rm -f $(docker ps -a -q)
 ```

## Deleting all images that are not being used by running containers
The additional filter is to do not delete recently downloaded images.
```bash
docker image prune -a --filter "until=24h"
```