# Docker Cheatsheet

## Quick Start
```bash
docker run hello-world
```

## Common Commands
```bash
docker build -t myapp .
docker images
docker ps -a
docker exec -it <container> /bin/bash
docker stop <container>
docker rm <container>
```

## Useful Snippets
```bash
# Remove all stopped containers
docker container prune

# Remove all unused images
docker image prune

# Tag and push to Docker Hub
docker tag myapp mlepe/myapp:latest
docker push mlepe/myapp:latest
```

## References
- [Docker Docs](https://docs.docker.com/)