Essential Docker commands that DevOps engineers commonly use in their day-to-day activities:

### Basic Commands

1. **Check Docker Version**

   ```sh
   docker --version
   ```
   Displays the installed version of Docker.

2. **List Docker Images**
   
   ```sh
   docker images
   ```
   Lists all Docker images on the local machine.

3. **Pull Docker Image**
   
   ```sh
   docker pull <image_name>
   ```
   Pulls a Docker image from a registry (e.g., Docker Hub).

4. **Remove Docker Image**
   
   ```sh
   docker rmi <image_name>
   ```
   Removes a Docker image from the local machine.

### Container Management

5. **Run a Container**
   
   ```sh
   docker run -d --name <container_name> <image_name>
   ```
   Runs a container from a Docker image in detached mode.

6. **List Running Containers**
   
   ```sh
   docker ps
   ```
   Lists all running Docker containers.

7. **List All Containers**
   
   ```sh
   docker ps -a
   ```
   Lists all Docker containers, including stopped ones.

8. **Stop a Container**

   ```sh
   docker stop <container_name>
   ```
   Stops a running Docker container.

9. **Start a Container**
   
   ```sh
   docker start <container_name>
   ```
   Starts a stopped Docker container.

10. **Remove a Container**
    
    ```sh
    docker rm <container_name>
    ```
    Removes a Docker container.

11. **View Container Logs**
    
    ```sh
    docker logs <container_name>
    ```
    Displays the logs of a Docker container.

### Networking

12. **List Docker Networks**
    
    ```sh
    docker network ls
    ```
    Lists all Docker networks.

13. **Create Docker Network**
    
    ```sh
    docker network create <network_name>
    ```
    Creates a new Docker network.

14. **Connect Container to Network**
    
    ```sh
    docker network connect <network_name> <container_name>
    ```
    Connects a container to a Docker network.

15. **Disconnect Container from Network**
    
    ```sh
    docker network disconnect <network_name> <container_name>
    ```
    Disconnects a container from a Docker network.

### Volumes

16. **List Docker Volumes**
    
    ```sh
    docker volume ls
    ```
    Lists all Docker volumes.

17. **Create Docker Volume**

    ```sh
    docker volume create <volume_name>
    ```
    Creates a new Docker volume.

18. **Remove Docker Volume**

    ```sh
    docker volume rm <volume_name>
    ```
    Removes a Docker volume.

19. **Mount Volume to Container**
    
    ```sh
    docker run -d --name <container_name> -v <volume_name>:/path/in/container <image_name>
    ```
    Mounts a Docker volume to a container.

### Docker Compose

20. **Install Docker Compose**
    
    ```sh
    sudo curl -L "https://github.com/docker/compose/releases/download/<version>/docker-compose-$(uname -s)-$(uname -m)" -o 
    /usr/local/bin/docker-compose
    ```
    Installs Docker Compose.

21. **Run Docker Compose**
    
    ```sh
    docker-compose up
    ```
    Starts services defined in a `docker-compose.yml` file.
