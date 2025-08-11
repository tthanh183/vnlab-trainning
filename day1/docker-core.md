# Day 1

## Docker

Docker is an open-source platform that helps develop, deliver, and run applications by packaging them into lightweight, portable containers. These containers isolate applications from infrastructure, enable fast deployment, improve security through isolation, and ensure consistency across environments.

## Docker Architecture

Docker follows a client-server architecture consisting of three main components: Docker Client, Docker Host, and Docker Registry.

![Screenshot of a Docker Architecture.](https://www.tutorialspoint.com/docker/images/docker_architecture.jpg)

1.  Docker Client: The Docker Client is the interface that users interact with (e.g., command line tools like `docker run`, `docker build`, etc.).  
    It sends requests to the Docker Daemon through a REST API, using UNIX sockets or network communication.

2.  Docker Host:  
    The Docker Host is the system (physical or virtual) that runs the Docker Daemon (dockerd), which is responsible for:

    - Managing containers, images, volumes, networks, etc.
    - Running containerized applications.
    - Communicating with Docker clients and registries.

      Key components inside the Docker Host:

    - Docker Daemon (dockerd): Core service that listens to client commands and manages containers, images, networks, and volumes.
    - Containers: Running instances of Docker images, providing isolated environments for executing applications.
    - Images: Read-only templates used to create containers; built from Dockerfiles and stored locally or in registries.

3.  Docker Registry:
    The Docker Registry is a storage and distribution system for Docker images.
    - Public Registry: Docker Hub (default)
    - Private Registry: Can be hosted in-house

## Docker Image

### What are Docker Images?

A Docker image is a read-only template that contains instructions for creating a container. It is usually built on top of a base image (like Ubuntu), with added components such as a web server, application code, and specific configurations.

To build custom images, a Dockerfile is used, where each instruction creates a new image layer. When the image is rebuilt, only the layers that have changed are updated. This layered structure makes Docker images lightweight, efficient, and fast to build and deploy.

### Key Components and Concepts

1. Layers:

- Docker images consist of several layers. Every layer denotes a collection of filesystem modifications. Each Dockerfile instruction adds a layer on top of the previous one while building a Docker image.

- Layers are unchangeable once they are produced, which makes them immutable. Because of its immutability, Docker can effectively reuse layers during image builds and deploys, which speeds up build times and uses less disk space.

2. Base Image

- Dockerfile is a text document with a set of instructions for creating a Docker image. These instructions describe how to create the basic image, add files and directories, install dependencies, adjust settings, and define the container's entry point.

3. Image Registry

- Docker images can be stored in either public or private registries Registries offer a centralized area for managing, sharing, and distributing Docker images. They also provide image scanning for security flaws, versioning, and access control.

4. Tagging

- A repository name and a tag combine to form a unique identification for Docker images. Tags are used to distinguish between various image versions. When no tag is given, Docker uses the "latest" tag by default. To maintain reproducibility and track image versions, it is recommended to utilize semantic versioning or other relevant tags.

5. Layer Caching

- For performance optimization, Docker uses layer caching while building images. When you construct an image, Docker leverages previously built cached layers if the associated Dockerfile instructions haven't changed. This drastically cuts down on build times, particularly for big projects with intricate dependencies.

### Docker Image Commands

- Listing all Docker Images
  > docker images
- Pulling Docker Images
  > docker pull <image_name>:<tag>
- Building Docker Images from Dockerfile
  > docker build -t <image_name>:<tag> <path_to_dockerfile_directory>
- Tagging Docker Images
  > docker tag <source_image>:<source_tag> <target_repo>/<target_image>:<target_tag>
- Pushing Docker Images _(ensure that you have signed in to the registry with the `docker login` command)_
  > docker push <target_repo>/<target_image>:<target_tag>
- Removing Docker Images
  > docker rmi <image_name>:<tag>
- Pruning Docker Images
  > docker image prune -a

## Docker Container

### What are Docker Containers?

A Docker container is a running instance of a Docker image. Docker allows you to create, run, kill, move, or delete a container. A Docker container is based on the associated image and the configurations provided when you start the container. When you stop or remove a container, changes that were not persisted to persistent storage such as volumes, etc. would be lost.

### Key Components and Concepts

1. Containerization

- Containers function based on the concept of containerization, which is packing an application together with all of its dependencies into a single package. This package, referred to as a container image, includes all of the necessary runtime environments, libraries, and other components needed to run the application.

2. Isolation

- Operating system-level virtualization is used by Docker containers to offer application isolation. With its filesystem, network interface, and process space, each container operates independently of the host system as a separate process.

- By maintaining their independence from one another, containers are kept from interfering with one another's operations thanks to this isolation.

3. Docker Engine

- The Docker Engine is the brains behind Docker containers; it builds, launches, and maintains them. The Docker daemon, which operates in the background, and the Docker client, which lets users communicate with the Docker daemon via commands, are two of the parts that make up the Docker Engine.

4. Image and Container Lifecycle

The image can be used to instantiate containers, which are instances of the image that are running after it has been created. It is possible to start, stop, pause, and restart containers as one.

### Docker Container Lifecycle

There are five essential phases in the Docker container lifecycle: created, started, paused, exited, and dead. The lifecycle of a container is represented by its stages, which range from creation and execution to termination and possible recovery.

![Screenshot of a Docker Container Lifecycle.](https://www.tutorialspoint.com/docker/images/docker_containers_1.jpg)

1. The Created State

- The "created" state is the first stage. When a container is created with the docker create command or a comparable API call, it reaches this phase. The container is not yet running when it is in the "created" state, but it does exist as a static entity with all of its configuration settings defined.

- At this point, Docker reserves the storage volumes and network interfaces that the container needs, but the processes inside the container have not yet begun.

2. The Started State

- The "started" or "running" state is the next stage of the lifecycle. When a container is started with the docker start command or an equivalent API call, it enters this stage.

- When a container is in the "started" state, its processes are launched and it starts running the service or application that is specified in its image. While they carry out their assigned tasks, containers in this state actively use CPU, memory, and other system resources.

3. The Paused State

- Throughout their lifecycle, containers may also go into a "paused" state. When a container is paused with the docker pause command, its processes are suspended, thereby stopping its execution.

- A container that is paused keeps its resource allotments and configuration settings but is not in use. This state helps with resource conservation and debugging by momentarily stopping container execution without completely stopping it.

4. The Exited State

- A container in the "exited" state has finished executing and has left its primary process. Containers can enter this state when they finish the tasks they are intended to complete or when they run into errors that force them to terminate.

- A container that has been "exited" stays stopped, keeping its resources and configuration settings but ceasing to run any processes. In this condition, containers can be completely deleted with the docker rm command or restarted with the docker start command.

5. The Dead State

- A container that is in the "dead" state has either experienced an irreversible error or been abruptly terminated. Critical errors in the containerized application, problems with the host system underneath, or manual intervention can all cause containers to enter this state.

- When a container is in the "dead" state, it is not in use and the Docker daemon usually releases or reclaims its resources. To free up system resources, containers in this state need to be deleted using the docker rm command since they cannot be restarted.

### Docker Container Commands

- Listing all Docker Containers
  > docker ps
- Running a Docker Container
  > docker run -d -p <host_port>:<container_port> <image_name>:<tag>
- Stopping a Docker Container
  > docker stop <container_id_or_name>
- Pausing a Running Container
  > docker pause <container_id_or_name>
- Resuming a Docker Container
  > docker unpause <container_id_or_name>
- Restarting a Container
  > docker restart <container_id_or_name>
- Executing Commands in a Running Docker Container
  > docker exec -it <container_id_or_name> <command>
- Removing a Docker Container
  > docker rm <container_id_or_name>
