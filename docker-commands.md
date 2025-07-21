### 🚢 Docker Commands 🐳

Once you deal with Kubernetes environment, it is better to remember some of the basic Docker commands.

#### Docker Engine Commands

- Listing local images:
```code
docker images
```

- Pulling the Docker image from the registry
```code
docker pull <image-name>
```

- Building image from the Dockerfile
```code
docker build -t <image-name> .
```

- Removing an image
```code
docker rmi <image-name>
```

#### Docker Container Commands

- Running the container:
```code
docker run <image-name>  
```

- Running the container in detached (background) mode:
```code
docker run -d <image-name>
```

- Running the container with name and interactive terminal
```code
docker run -it --name <container-name> <image-name>
```

- Mapping host port to container port:
```code
docker run -p 8080:80 <image-name>
```
<i>Note that 8080 is host port and 80 is the port inside the Docker</i>

- Running a container with a custom name
```code
docker run --name <name> image-name 
```

- Starting a stopped container
```code
docker start <container-id/name>
```

- Stopping a started container
```code
docker stop <container-id/name>
```

- Restarting a container
```code
docker restart <container-id/name>
```

- Deleting a stopped container
```code
docker rm <container-id/name>
```

- Listing all the containers
```code
docker ps
```

- Listing all running containers
```code
docker ps -a
```
