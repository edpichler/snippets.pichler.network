# Some devcontainer ready images
 - mcr.microsoft.com/devcontainers/java:17
 - More in https://github.com/devcontainers/images/tree/main/src

# Dev Container metadata reference
https://containers.dev/implementors/json_reference/

# Light RUN apt install commands in Dockerfile
https://github.com/devcontainers/images/blob/main/docs/TIPS.md/#why-do-dockerfiles-in-this-repository-use-run-statements-with-commands-separated-by

# Build and publish the container image
```

$GIT_HASH=git rev-parse --short=7 HEAD
docker build -t edpichler/devcontainer-hugo:latest -t edpichler/devcontainer-hugo:$GIT_HASH .
docker push edpichler/devcontainer-hugo:latest
docker push edpichler/devcontainer-hugo:$GIT_HASH
```
