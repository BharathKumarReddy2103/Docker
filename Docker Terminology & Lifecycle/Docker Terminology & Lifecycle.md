**Docker Terminology & Lifecycle**

**Introduction**

Welcome to the **Docker** guide This repository is designed to help you understand Docker fundamentals, from basic concepts to advanced usage.

This guide covers:

•	Why containers are lightweight

•	Docker lifecycle and architecture

•	Key Docker terminologies

•	Installing Docker on an AWS EC2 instance

•	Common Docker setup mistakes and fixes

•	Writing your first Dockerfile and running a container

•	Pushing Docker images to a public registry

If you find this repository helpful, **consider giving it a star ⭐**

---

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

---

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
