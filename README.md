# Docker Basics

This README provides an introduction to Docker, including its key concepts, basic commands, architecture, and a simple example to get started.

---

## Table of Contents

1. [What is Docker?](#what-is-docker)
2. [Docker Architecture](#docker-architecture)
3. [Basic Docker Concepts](#basic-docker-concepts)
4. [Installing Docker](#installing-docker)
5. [Basic Docker Commands](#basic-docker-commands)
6. [Example: Running a Simple Web App](#example-running-a-simple-web-app)
7. [Docker Compose](#docker-compose)
8. [Further Resources](#further-resources)

---

## What is Docker?

Docker is an open-source platform used to automate the deployment, scaling, and management of applications within lightweight, portable containers. It allows developers to package an application and its dependencies into a single unit, ensuring consistent behavior across different environments (development, testing, production).

---

## Docker Architecture

Docker follows a client-server architecture:

1. **Docker Client**: The client is the interface where you interact with Docker. It allows you to issue commands to the Docker Daemon (e.g., `docker run`, `docker build`).

2. **Docker Daemon**: The daemon (`dockerd`) is responsible for managing Docker containers and images. It listens for requests from the Docker client and manages container lifecycles.

3. **Docker Images**: Docker images are read-only templates used to create containers. They contain everything needed to run an application (code, runtime, libraries, environment variables).

4. **Docker Containers**: Containers are lightweight, standalone packages that contain everything needed to run a specific application. Containers are created from Docker images and can run on any system with Docker installed.

5. **Docker Registry**: The registry is where Docker images are stored and distributed. The default public registry is Docker Hub, but you can also set up private registries.

   ![Docker Architecture Diagram](https://www.docker.com/sites/default/files/d8/2019-09/Intro-to-Docker-Architecture.png)


---

## Basic Docker Concepts

- **Container**: A lightweight, portable unit that runs an application in an isolated environment. Containers share the host OS kernel but are otherwise isolated.

- **Image**: A template or snapshot of a container that contains the file system and parameters required to run the container.

- **Volume**: A persistent storage mechanism that allows data to be shared between containers or between a container and the host machine.

- **Network**: Docker allows you to create isolated networks for containers to communicate with each other.

---

## Installing Docker

### For Windows:
1. Download Docker Desktop from the official [Docker website](https://www.docker.com/products/docker-desktop).
2. Follow the installation instructions and ensure that Docker Desktop is running.

### For Mac:
1. Download Docker Desktop for Mac from [Docker's website](https://www.docker.com/products/docker-desktop).
2. Install the application and follow the setup instructions.

## Containers vs Virtual Machine

Containers and virtual machines are both technologies used to isolate applications and their dependencies, but they have some key differences:

    1. Resource Utilization: Containers share the host operating system kernel, making them lighter and faster than VMs. VMs have a full-fledged OS and hypervisor, making them more resource-intensive.

    2. Portability: Containers are designed to be portable and can run on any system with a compatible host operating system. VMs are less portable as they need a compatible hypervisor to run.

    3. Security: VMs provide a higher level of security as each VM has its own operating system and can be isolated from the host and other VMs. Containers provide less isolation, as they share the host operating system.

4.  Management: Managing containers is typically easier than managing VMs, as containers are designed to be lightweight and fast-moving.



### Docker LifeCycle

We can use the above Image as reference to understand the lifecycle of Docker.

There are three important things,

1. docker build -> builds docker images from Dockerfile
2. docker run   -> runs container from docker images
3. docker push  -> push the container image to public/private regestries to share the docker images.

![Screenshot 2023-02-08 at 4 32 13 PM](https://user-images.githubusercontent.com/43399466/217511949-81f897b2-70ee-41d1-b229-38d0572c54c7.png)

#### Dockerfile

Dockerfile is a file where you provide the steps to build your Docker Image.


#### Images

An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

### Build your first Docker Image

You need to change the username accoringly in the below command

```
docker build -t vijsinha/my-first-docker-image:latest .
```

Output of the above command

```
    Sending build context to Docker daemon  992.8kB
    Step 1/6 : FROM ubuntu:latest
    latest: Pulling from library/ubuntu
    677076032cca: Pull complete
    Digest: sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f
    Status: Downloaded newer image for ubuntu:latest
     ---> 58db3edaf2be
    Step 2/6 : WORKDIR /app
     ---> Running in 630f5e4db7d3
    Removing intermediate container 630f5e4db7d3
     ---> 6b1d9f654263
    Step 3/6 : COPY . /app
     ---> 984edffabc23
    Step 4/6 : RUN apt-get update && apt-get install -y python3 python3-pip
     ---> Running in a558acdc9b03
    Step 5/6 : ENV NAME World
     ---> Running in 733207001f2e
    Removing intermediate container 733207001f2e
     ---> 94128cf6be21
    Step 6/6 : CMD ["python3", "app.py"]
     ---> Running in 5d60ad3a59ff
    Removing intermediate container 5d60ad3a59ff
     ---> 960d37536dcd
    Successfully built 960d37536dcd
    Successfully tagged vijsinha/my-first-docker-image:latest
```

### Verify Docker Image is created

```
docker images
```

Output

```
REPOSITORY                         TAG       IMAGE ID       CREATED          SIZE
vijsinha/my-first-docker-image   latest    960d37536dcd   26 seconds ago   467MB
ubuntu                             latest    58db3edaf2be   13 days ago      77.8MB
hello-world                        latest    feb5d9fea6a5   16 months ago    13.3kB
```

### Run your First Docker Container

```
docker run -it vijsinha/my-first-docker-image
```

Output

```
Hello World
```

### Push the Image to DockerHub and share it with the world

```
docker push vijsinha/my-first-docker-image