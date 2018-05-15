# Docker Cheat Sheet

### Building a Docker image
```
docker build -t my-image .
```

### Running a Docker container
```
docker run -d --name my-container -t my-image
```

### Running a shell in a Docker image
Works with Alpine builds.
```
docker run -it my-image /bin/sh
```

### Cleaning up Docker images and containers
```
docker system prune -a
```
The `-a` option removes _unused_ objects, as well as dangling.

Upon running this command on my system for the first time, Docker freed up _11GB of space_. So yeah, run this command.

### Stop a Docker container

If you `kill` a container, it counts as failing, i.e. has a non-zero exit code.

```
docker stop my-container-id
```
