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
The additional filter is to do not delete images downloaded less than 2 days ago.
```bash
 docker system prune -a --filter "until=$((2*24))h"
```
If you want to add on crontab, the -f flag will just prevent of the confirmation prompt be showed during the cron job running:
```bash
 docker system prune -af --filter "until=$((2*24))h"
```

# Docker Swarm

## Make a node inactive/active
It will stops imediatelly all running containers in that node. They will be balanced to other nodes, though (if its configuration allows it).

``` bash 
docker node ls
docker node update --availability drain 15wkcqw42fjzb1aqoavyfkk13
docker node update --availability active 15wkcqw42fjzb1aqoavyfkk13
```

