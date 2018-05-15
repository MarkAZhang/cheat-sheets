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
