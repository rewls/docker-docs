# Docker overview

- Docker is an open platform for developing, shipping, and running applications.

- Docker enables you to separate your applications from your infrastructure so you can deliver software quickly.

## The Docker platform

- Docker provides the ability to package and run an application in a loosely isolated environment called a container.

- Docker provides tooling and a platform to manage the lifecycle of your containers:

    - Develop your application and its supporting components using containers.

    - The container becomes the unit for distributing and testing your application.

    - When you're ready, deploy your application into your production environment, as a container or an orchestrated service.

## What can I use Docker for?

- Fast, consistent delivery of your applications

- Responsive deployment and scaling

- Running more workloads on the same hardware

## Docker architecture

- Docker uses a client-server architecture.

- The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers.

- The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon.

- The Docker client and daemon communicate using a REST API, over UNIX sockets or a network Interface.m:w

- Another Docker client is Docker Compose,  that lets you work with applications consisting of a set of containers.

### The Docker daemon

- The Docker daemon (`dockerd`) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes.

- A daemon can also communicate with other daemons to manage Docker services.

### The Docker client

- The Docker client (`docker`) is the primary way that many Docker users interact with Docker.

- The `docker` command uses the Docker API.

- The Docker client can communicates with more than one daemon.

### Docker registries

- A Docker registry stores Docker images.

- Docker Hub is a public registry that anyone can use, and Docker looks for images on Docker Hub by default.

- When you use the `docker pull` or `docker run` commands, Docker pulls the required images from your configured registry.

- When you use the `docker push` command, Docker pushes your image to your configured registry.

### Docker objects

#### Images

- An image is a read-only template with instructions for creating a Docker container.

- To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it.

- Each instruction in a Dockerfile creates a layer in the image.

- When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt.

#### Containers

- A container is a runnable instance of an image.

- You can create, start, stop, move, or delete a container using the Docker API or CLI.

- You can connect a container to ones or more networks, attach storage to it, or even create a new image based on its current state.

- You can control how isolated a container's network, storage, or other underlying subsystems are from other containers or from the host machine.

- A container is defined by its image as well as any configuration options you provide to it when you create or start it.

- When a container is removed, any changes to its state that aren't stored in persistent storage disappear.

##### Example `docker run` command

- The following command runs an `ubuntu` container, attaches interactively to your local command-line session, and runs `/bin/bash`.

- When you run this command, the following happens:

    1. If you don't have the `ubuntu `image locally, Docker pulls it from your configured registry, as though you had run `docker pull ubuntu` manually.

    2. Docker creates a new container, as though you had run a `docker container create` command manually.

    3. Docker allocates a read-write filesystem to the container, as its final layer.

    4. Docker creates a network interface to connect the container to the default network.

        - By default, containers can connect to external networks using the host machine's network connection.

    5. Dockerstarts the container and executes `/bin/bash`.

        - Because the container is running interactively and attached to your terminal (due to the `-i` and `-t` flags), you can provide input using your keyboard while Docker logs the output to your terminal.

    6. When you run `exit` to terminate the `/bin/bash` command, the container stops but isn't removed.

## The underlying technology

- Docker is written in the Go programming language and takes advantage of several features of the Linux kernel to deliver its functionality.

- Docker uses a technology called `namespaces` to provide the isolated workspace called the container.

- When you run a container, Docker creates a set of namespaces for that container.

- These namespaces provide a layer of isolation.

## Next steps

- Install Docker

- Get started with Docker
