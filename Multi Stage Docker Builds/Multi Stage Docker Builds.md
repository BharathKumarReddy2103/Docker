**Optimizing Docker Images with Multi-Stage Builds and Distroless Images**

**Introduction**

This guide explains how to optimize Docker images using **Multi-Stage Docker Builds** and **Distroless Images**. These techniques help in reducing image sizes, improving security, and optimizing containerized applications.

**Concept of Multi-Stage Docker Builds**

A **multi-stage build** is a method to create lightweight production-ready Docker images by separating the **build process** from the **runtime execution**.

**Traditional Dockerfile (Without Multi-Stage Build)**

A basic example for running a **GoLang-based calculator application** without multi-stage builds:

```sh
FROM ubuntu:latest  
RUN apt-get update && apt-get install -y golang  
WORKDIR /app  
COPY calculator.go /app  
RUN go build -o calculator  
CMD ["./calculator"]
```

**Problems with the Traditional Approach**

**1.	Large Image Size** – Includes unnecessary OS dependencies.

**2.	Security Risks** – Exposes the container to vulnerabilities.

**3.	Longer Build Times** – Increases deployment overhead.

After building the image:

```sh
docker build -t simple-calculator .
docker images
```

The resulting image is **861MB**, which is inefficient for a simple calculator application.

---

**Optimizing with Multi-Stage Builds**

To reduce the image size, we use **Multi-Stage Builds**.

**Refactored Dockerfile (Using Multi-Stage Build)**

```sh
# Stage 1: Build Stage
FROM ubuntu:latest AS build  
RUN apt-get update && apt-get install -y golang  
WORKDIR /app  
COPY calculator.go /app  
RUN go build -o calculator  

# Stage 2: Minimal Production Image
FROM scratch  
COPY --from=build /app/calculator /calculator  
CMD ["/calculator"]
```

**How It Works**

**1.	Stage 1 (Build Stage)**

o	Uses Ubuntu to install dependencies and compile the Go application.

o	Generates a binary (calculator).

**2.	Stage 2 (Final Image)**

o	Uses **scratch** (a minimal image with no OS dependencies).

o	Only copies the **compiled binary** from the first stage.

After building:

```sh
docker build -t optimized-calculator .
docker images
```

The final image size is only **1.83MB** – a 99% reduction from 861MB.

---

**What Are Distroless Images?**

**Distroless images** are minimal base images that only contain the necessary runtime environment for an application. Unlike traditional images (Ubuntu, Alpine), they **exclude**:

•	Package managers (apt, yum)

•	Shells (bash, sh)

•	System utilities (curl, wget)

**Why Use Distroless Images?**

**✔ Security** – Fewer attack surfaces, reducing vulnerabilities.

**✔  Smaller Size** – No unnecessary dependencies.

**✔ Performance** – Faster builds and efficient runtime execution.

**Using Distroless Images**

To use a **distroless image** for Java applications, update the Dockerfile:

```sh
FROM gcr.io/distroless/java11  
COPY app.jar /app.jar  
CMD ["java", "-jar", "/app.jar"]
```

For more distroless images, check: Google Distroless GitHub.

---

**Key Takeaways**

✅ Multi-Stage Builds **reduce image size** and **improve efficiency**.

✅ Distroless Images **enhance security** by eliminating unnecessary packages.

✅ These techniques help in **faster deployments and better resource utilization**.

---

**Conclusion**

By adopting **Multi-Stage Docker Builds** and Distroless Images, you can:

•	Reduce **Docker image size** significantly (**861MB → 1.83MB**).

•	Improve **container security** by minimizing attack surfaces.

•	Optimize **performance** by reducing unnecessary dependencies.
