**Introduction to Containers**

**Overview**

This document provides an introduction to **containers** as a modern approach to software deployment and resource management. It explains the **differences between virtual machines and containers, the advantages of containerization**, and introduces **Docker** and **Buildah**, two popular container tools.

---

**What Are Containers?**

Containers are a **lightweight** and **efficient** alternative to virtual machines. They allow applications to run in isolated environments while sharing the underlying operating system resources. This makes them **faster, more efficient**, and **easier to deploy** compared to traditional VMs.

**Why Do We Need Containers?**

Before understanding containers, it's important to know about **virtual machines (VMs)**:

1.	A **physical server** (like a laptop or data center machine) has a fixed amount of resources (CPU, RAM, storage).
  
2.	**Virtualization** allows **multiple virtual machines (VMs)** to run on a single physical server using a **hypervisor**.
  
3.	Each VM has its **own operating system**, which provides isolation but also consumes a lot of resources.

**Problem with VMs**:

•	Each VM includes a full OS, which increases overhead.

•	Many VMs waste resources because applications rarely use the full capacity allocated to them.

**Solution: Containers**

•	Containers **share** the host OS instead of having a full OS for each instance.

•	This makes containers **lighter, faster, and more efficient**.

•	Containers help organizations **save costs** by maximizing resource utilization.

---

**Virtual Machines vs. Containers**

## Virtual Machines vs. Containers

| Feature            | Virtual Machines (VMs)    | Containers              |
|-------------------|------------------------|------------------------|
| OS Isolation     | Each VM has a full OS   | Shares host OS         |
| Resource Usage   | High (due to OS overhead) | Low (lightweight)      |
| Boot Time       | Slow (minutes)          | Fast (seconds)         |
| Portability     | Difficult               | Easy to move & deploy  |

**How Containers Work**

Containers **package** an application along with its dependencies in an isolated environment. This allows it to **run consistently** across different platforms.

There are **two ways** to deploy containers:

**1.	Directly on a physical server**

     o	Install a **container runtime** like Docker or Podman.

o	Run multiple containers on top of the OS.

**2.	On top of a Virtual Machine (VM)**

o	Install a container runtime inside a VM.

o	Run multiple containers inside the VM.

o	This is common in **cloud environments** like AWS, Azure, and GCP.

Most organizations use the **second approach** (containers inside VMs) because cloud providers manage the physical infrastructure, reducing maintenance efforts.

---

**Introduction to Docker**

Docker is one of the most popular container platforms. It simplifies **creating, managing, and running containers**.

**Docker Workflow**:

**1.	Write a Dockerfile** – Defines how to build a container image.

**2.	Build the Image** – Converts the Dockerfile into a reusable image.

```sh
docker build -t my-app .
```

**3.	Run a Container** – Starts a container from the image.

```sh
docker run -d -p 8080:80 my-app
```

---

**Introduction to Buildah**

While Docker is widely used, **Buildah** is another tool that provides a **more flexible way** to create container images. Unlike Docker, Buildah:

•	Does **not require a Docker daemon** (no single point of failure).

•	Allows **more control** over how images are built.

•	Works well with **Podman and Skopeo**.

With Buildah, you can create images using simple shell scripts instead of writing a Dockerfile.

---

**Key Takeaways**

•	**Containers** solve resource wastage issues in VMs by sharing the host OS.

•	**Docker** is the most popular tool for managing containers.

•	**Buildah** offers a lightweight, daemon-less alternative to Docker.

•	Containers are **portable, fast**, and **cost-efficient** for modern applications.

