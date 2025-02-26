**Docker Terminology & Lifecycle**

**Introduction**

Welcome to the **Docker** guide. This repository is designed to help you understand Docker fundamentals, from basic concepts to advanced usage.

This guide covers:

• What is a container

•	Why containers are lightweight

•	Docker lifecycle and architecture

•	Key Docker terminologies

•	Installing Docker on an AWS EC2 instance

•	Common Docker setup mistakes and fixes

•	Writing your first Dockerfile and running a container

•	Pushing Docker images to a public registry

If you find this repository helpful, **consider giving it a star ⭐**

---

**What is a container ?**

A container is a bundle of Application, Application libraries required to run your application and the minimum system dependencies.

**Containers vs Virtual Machine**

1. Resource Utilization: Containers share the host operating system kernel, making them lighter and faster than VMs. VMs have a full-fledged OS and hypervisor, making them more resource-intensive.

2. Portability: Containers are designed to be portable and can run on any system with a compatible host operating system. VMs are less portable as they need a compatible hypervisor to run.

3. Security: VMs provide a higher level of security as each VM has its own operating system and can be isolated from the host and other VMs. Containers provide less isolation, as they share the host operating system.

**Why Are Containers Lightweight?**

Unlike Virtual Machines (VMs), **containers do not have a full guest operating system**. Instead, they only contain:

•	Application code

•	Application dependencies

•	Minimal system dependencies

Everything else, including the **kernel, file system**, and **networking stack**, is shared with the host operating system. This makes containers significantly more lightweight.

**Example Comparison:**

| **Platform**            | **Size**       |
|-------------------------|---------------|
| Ubuntu Virtual Machine  | ~2.3 GB       |
| Ubuntu Docker Image     | ~28 MB        |

Since containers share resources with the host OS, you can run **multiple containers** on a single VM efficiently.

**Files and Folders in containers base images**

    /bin: contains binary executable files, such as the ls, cp, and ps commands.

    /sbin: contains system binary executable files, such as the init and shutdown commands.

    /etc: contains configuration files for various system services.

    /lib: contains library files that are used by the binary executables.

    /usr: contains user-related files and utilities, such as applications, libraries, and documentation.

    /var: contains variable data, such as log files, spool files, and temporary files.

    /root: is the home directory of the root user.

**Files and Folders that containers use from host operating system**

• The host's file system: Docker containers can access the host file system using bind mounts, which allow the container to read and write files in the host file system.

• Networking stack: The host's networking stack is used to provide network connectivity to the container. Docker containers can be connected to the host's network directly or through a virtual network.

• System calls: The host's kernel handles system calls from the container, which is how the container accesses the host's resources, such as CPU, memory, and I/O.

• Namespaces: Docker containers use Linux namespaces to create isolated environments for the container's processes. Namespaces provide isolation for resources such as the file system, process ID, and network.

• Control groups (cgroups): Docker containers use cgroups to limit and control the amount of resources, such as CPU, memory, and I/O, that a container can access.
    
---

**Docker**

**What is Docker** ?

Docker is a containerization platform that provides easy way to containerize your applications, which means, using Docker you can build container images, run the images to create containers and also push these containers to container regestries such as DockerHub, Quay.io and so on.

In simple words, you can understand as containerization is a concept or technology and Docker Implements Containerization.

**Docker Lifecycle & Architecture**

**Docker Lifecycle**:

**1.	Write a Dockerfile**

A file that defines the steps to containerize an application.

**2.	Build a Docker Image**

Convert the Dockerfile into an image using docker build.

**3.	Run a Docker Container**

Create a running instance using docker run.

**4.	Push to Docker Registry**

Share the Docker image using docker push.

**Docker Architecture:**

Docker consists of three main components:

**•	Docker Daemon**: Core background service managing images and containers.

**•	Docker CLI**: The command-line tool used to interact with Docker.

**•	Docker Registry**: A storage hub for sharing Docker images (e.g., Docker Hub, AWS ECR).

---

**Key Docker Terminologies**

| **Term**              | **Definition** |
|----------------------|--------------|
| **Docker Daemon**    | The background service that runs containers and manages images. |
| **Docker Client (CLI)** | The command-line tool used to interact with Docker. |
| **Docker Registry**  | A storage hub for sharing Docker images (e.g., Docker Hub, AWS ECR). |
| **Docker Hub**       | A public registry where Docker images are stored and shared. |
| **Docker Image**     | A pre-built snapshot of an application, including dependencies. |
| **Docker Container** | A running instance of a Docker image. |
| **Dockerfile**       | A text file containing instructions to create a Docker image. |
| **Docker Compose**   | A tool for defining and running multi-container applications. |
| **Docker Networking** | Allows containers to communicate with each other and external networks. |
| **Docker Volume**    | A way to persist data in Docker containers across restarts. |

**Installing Docker on AWS EC2**

Follow these steps to install Docker on an Ubuntu-based AWS EC2 instance:

```sh
sudo apt update
sudo apt install docker.io -y
sudo systemctl status docker
```

To avoid permission errors, add your user to the Docker group:

```sh
sudo usermod -aG docker $USER
logout  # Log out and log in again
```

Verify Docker installation:

```sh
docker run hello-world
```

---

**Writing Your First Dockerfile**

Create a **simple Python application** inside a file named app.py:

```sh
print("Hello, World!")
```

Now, create a Dockerfile:

```sh
FROM ubuntu:latest
WORKDIR /app
COPY . /app
RUN apt update && apt install python3 -y
CMD ["python3", "app.py"]
```

**Build & Run Your First Docker Container:**

```sh
docker build -t my-python-app .
docker run my-python-app
```

You should see:

```sh
Hello, World!
```

---

**Pushing Docker Images to Docker Hub**

To share your Docker image with others:

1.	Create an account on Docker Hub.
  
2.	Log in to Docker Hub:

```sh
docker login
```

1.	Tag and push your image:

```sh
docker tag my-python-app username/my-python-app:latest
docker push username/my-python-app:latest
```

Now, anyone can pull and run your image:

```sh
docker pull username/my-python-app:latest
docker run username/my-python-app
```

---

**Why Use Docker?**

•	**Portability** – Run applications anywhere.

•	**Lightweight** – Containers use minimal resources.

•	**Scalability** – Easily scale applications.

•	**Consistency** – Works the same in development and production.

•	**Faster Deployments** – Avoid manual environment setup.
