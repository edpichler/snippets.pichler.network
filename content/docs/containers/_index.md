---
title: "Containers (docker)"
weight: 3
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: false
# bookSearchExclude: false
---
# Containers (docker)
Containerization and virtualization is the ultimate separation of concerns. The breakdown of complexity and the de-coupling of the system components. 

## Running a container exposing a port:
Exposes the container `80` port as a local/host `8080`:
 ```bash
 docker run -p 8080:80 edpichler/nginx
 ```

 ## Entering inside a running docker container

 ``` bash
    docker exec -it e4383e55958d /bin/bash
 ```
## Listing the installed packages in a container
And dumping the output to a file:
 ``` bash
    docker run --rm ubuntu dpkg -l > installed_libraries.txt
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

## Update a service

```bash 
docker service list

docker service update --force myservice-api --image myname/myimage:latest --update-order start-first --update-failure-action rollback --with-registry-auth  --replicas 1 --env-add FLYWAY_MIGRATE="false"
```

## Kill a service
``` bash
docker service list

docker service update --force myservice-api --replicas 0 
```

## Add certificates to images

In the `Dockerfile`

```
# Install the ca-certificate package
RUN apt-get update && apt-get install -y ca-certificates

# Copy the CA certificate from the context to the build container
COPY your_certificate.crt /usr/local/share/ca-certificates/

# Update the CA certificates in the container
RUN update-ca-certificates
```
Notice: you need to have .crt certificates. If you have .pem, you can [corvert it with OpenSSL](https://snippets.pichler.network/docs/openssl/)

Source: https://docs.docker.com/engine/network/ca-certs/