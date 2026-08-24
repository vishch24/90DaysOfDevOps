# Introduction to Docker

## What is Docker?
Docker is an open-source platform that uses operating system-level virtualization to deliver software in lightweight, isolated packages called containers.
  
## What is a container and why do we need them?
- A **container** is a live, runnable instance of an image(blueprint) that bundles an application code, libraries and dependencies together.
- It helps to exclude environmental configuration indifference between the development and production teams.
- It helps to run dozens of isolated apps on a single machine without wasting system memory.
- It helps to split a large application into smaller chunks, i.e, microservices.
- It helps in rapid development on scaling and deploying applications instantly across cloud networks.

## Containers vs Virtual Machines — what's the real difference?

| Feature | Containers | Virtual Machines |
| :--- | :--- | :--- |
| **Architecture** | Shares the host OS kernel | Includes a full guest OS |
| **Size** | Megabytes (very lightweight) | Gigabytes (heavy) 
| **Boot Time** | Seconds (instant) | Minutes |
| **Performance** | Near-native speed | Higher resource overhead |

## What is the Docker architecture?

<img width="800" height="400" alt="image" src="https://docs.docker.com/get-started/images/docker-architecture.webp" />

**1. Docker daemon**
- The Docker daemon (`dockerd`) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes.
- A daemon can also communicate with other daemons to manage Docker services.

**2. Docker client**
- The Docker client (`docker`) is the primary way that many Docker users interact with Docker. 
- When you use commands such as `docker run`, the client sends these commands to `dockerd`, which carries them out.
- The `docker` command uses the Docker API. The Docker client can communicate with more than one daemon.

**3. Docker objects**
- When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.
  - **Image**
    - An image is a read-only template with instructions for creating a Docker container.
    - Often, an image is based on another image, with some additional customization.
    - For example, you may build an image that is based on the Ubuntu image but includes the Apache web server and your application, as well as the configuration details needed to make your application run.
  - **Containers**
    - A container is a runnable instance of an image. 
    - You can create, start, stop, move, or delete a container using the Docker API or CLI. 
    - You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

**4. Docker registries**
- A Docker registry stores Docker images.
- Docker Hub is a public registry that anyone can use, and Docker looks for images on Docker Hub by default.
- You can even run your own private registry.

---

