**Docker Networking**

**Introduction**

Docker networking is a key concept in containerized environments. It enables containers to communicate with each other and with the host system. This guide explains **Docker networking** concepts, different types of networking modes, and how to configure secure networking for containers.

**Why is Networking Important in Docker?**

Containers often need to:
**1.	Communicate with other containers** (e.g., frontend needs to talk to backend).

**2.	Be isolated from other containers** for security reasons (e.g., payment services should not be exposed to unauthorized users).

**How Docker Networking Works**

When a container is created, Docker assigns it to a network. By default:

•	The host system has a network interface (eth0).

•	Each container has a separate network interface with its own subnet.

•	Containers cannot directly communicate with the host due to network isolation.

Docker solves this by creating a **virtual bridge network** called docker0, which allows communication between the container and the host.

**Types of Docker Networking**

**1. Bridge Networking (Default)**

•	Containers communicate through a shared virtual bridge (docker0).

•	Each container gets an IP from the same subnet.

•	Used in most containerized applications.

**2. Host Networking**

•	The container **shares** the host's network.

•	No separate IP is assigned to the container.

•	Not recommended due to security risks.

**3. Overlay Networking**

•	Used for multi-host communication in Docker Swarm and Kubernetes.

•	Creates a virtual network across multiple hosts.

**4. None Networking**

•	The container has **no** network access.

•	Used for complete network isolation.

**Ensuring Security with Custom Bridge Networks**

By default, containers in the **bridge network** can communicate freely. However, some applications (e.g., payment services) require network isolation.

**Solution:** Create a **custom bridge network** to isolate sensitive containers.

**Steps to Configure Docker Networks**

**1. Check Existing Networks**

Run:

```sh
docker network ls
```

This will list:

•	bridge (default)

•	host

•	none

**2. Create a Container in the Default Bridge Network**

```sh
docker run -d --name app1 nginx
docker run -d --name app2 nginx
```

•	These containers will be assigned IPs within the same subnet.

•	Verify the IPs:

```sh
docker inspect app1 | grep "IPAddress"
docker inspect app2 | grep "IPAddress"
```

•	Containers in the same **bridge network** can communicate:

```sh
docker exec -it app1 ping <app2-IP>
```

**3. Create a Custom Bridge Network**

```sh
docker network create secure-net
```

Verify the network:

```sh
docker network ls
```

**4. Run a Secure Container**

```sh
docker run -d --name secure-app --network secure-net nginx
```

•	The secure-app container **cannot** communicate with app1 or app2.

•	Confirm by running:

```sh
docker exec -it app1 ping <secure-app-IP>
```

The request should fail.

**5. Create a Container with Host Networking**

```sh
docker run -d --name host-app --network host nginx
```

•	The container **does not get a separate IP**.

•	Run:

```sh
docker inspect host-app | grep "IPAddress"
```

You will see **no separate IP** because it uses the host's network.

**Conclusion**

Docker networking provides flexibility in **container communication and isolation**. Key takeaways:

**1.	Bridge Networking** allows containers to communicate.

**2.	Custom Bridge Networks** enable network isolation for security.

**3.	Host Networking** shares the host’s network (less secure).

**4.	Overlay Networking** is useful for multi-host setups.

**Next Steps**

•	Try out different networking configurations.

•	Experiment with Kubernetes networking.

•	Explore Docker Swarm for multi-host networking.

