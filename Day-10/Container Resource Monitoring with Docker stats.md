**Container Resource Monitoring with docker stats**

**Introduction**

Containerized applications are widely used in modern DevOps and cloud-native environments. Monitoring container resource usage is crucial for maintaining performance, ensuring optimal resource allocation, and troubleshooting performance bottlenecks.

Docker provides the docker stats command, a built-in tool that offers real-time resource utilization metrics for running containers. It helps developers and DevOps engineers monitor CPU, memory, network, and disk usage to optimize and troubleshoot containerized workloads.

In this guide, we will explore:

**•	How docker stats works**

**•	Understanding different metrics displayed**

**•	How to use docker stats with examples**

**•	Analyzing the output for performance monitoring**

**•	Real-world use cases**

**•	Automating monitoring using scripts**

**•	A comparison with other container monitoring tools**

---

**Understanding** docker stats

The docker stats command provides a real-time stream of resource usage statistics for containers. When executed, it displays the following key metrics:

| **Metric**         | **Description** |
|--------------------|----------------|
| **CPU Usage (%)**  | Percentage of total CPU resources used by the container. Can exceed 100% in multi-core systems. |
| **Memory Usage / Limit** | The actual memory being used vs. the maximum memory allocated for the container (if set). |
| **Memory Percentage (%)** | Memory usage relative to its limit. **Formula:** `(MEM USAGE / MEM LIMIT) * 100`. |
| **Network Traffic (NET I/O)** | Amount of data **received (RX) and transmitted (TX)** by the container. Helps detect spikes and bottlenecks. |
| **Block I/O (Disk Read/Write Operations)** | Shows disk activity in terms of **bytes read and written**. Useful for identifying excessive logging or database-intensive workloads. |
| **PIDs (Number of Processes)** | Displays the **number of active processes** inside the container. Can help detect process leaks or inefficiencies. |

---

**How to Use** docker stats

The docker stats command can be used in different ways:

**1. Monitor all running containers**

```sh
docker stats
```

Example Output:

```sh
CONTAINER ID   NAME       CPU %   MEM USAGE / LIMIT    MEM %   NET I/O     BLOCK I/O    PIDS
d3f1e22e0a11   web_app    2.56%   128MiB / 1GiB       12.5%    10kB / 5kB  2MB / 1MB    4
b6a9c91a9c12   db_server  1.78%   256MiB / 2GiB       12.8%    20kB / 10kB 4MB / 2MB    10
```

**2. Monitor a specific container**

```sh
docker stats <container_id_or_name>
```

Example:

```sh
docker stats web_app
```

**3. Display output in a custom format**

To extract specific fields using --format:

```sh
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"
```

Example Output:

```sh
NAME       CPU %   MEM USAGE
web_app    2.56%   128MiB / 1GiB
db_server  1.78%   256MiB / 2GiB
```

---

**Analyzing** docker stats **Output**

The docker stats output provides insights into container performance, helping to diagnose issues and optimize resource allocation.

**1. CPU % (CPU Usage)**

•	Indicates the **percentage of total CPU resources** utilized by the container.

•	On multi-core systems, values can exceed 100% (e.g., **200% means two full cores** being used).

**Interpreting CPU Usage Levels**

✅ **<10%** → Low usage, mostly idle
⚠️ **10-50%** → Moderate usage, typical for many workloads
🚨 >80%→ High CPU usage, potential bottleneck
  
💡 **Limit CPU usage for a container to prevent excessive resource consumption**:

```sh
docker run --cpus="1.5" your_container
```

---

**2. MEM USAGE / LIMIT (Memory Usage and Limits)**

•	**MEM USAGE** → Actual memory used by the container.

•	**MEM LIMIT** → Maximum memory allocated to the container (if defined). If not set, it shows the **total host memory**.

---

**3. MEM % (Memory Percentage Used)**

•	Represents **memory consumption relative to its limit.**

**•	Formula:**

```sh
•	Memory % = (MEM USAGE / MEM LIMIT) * 100
```

**Interpreting Memory Usage Levels**

✅ **<50%** → Optimal usage
⚠️ **50-80%** → Monitor closely
🚨 **>80%** → Risk of Out of Memory (OOM) errors

---

**4. NET I/O (Network Traffic)**

•	Displays the **amount of data received (RX) and transmitted (TX)** by the container.

•	Helps detect unexpected **traffic spikes or bottlenecks**.

**Interpreting Network Traffic**

✅ **Low network activity?** The application might be idle.
⚠️ **High network I/O?** Investigate excessive data transfer or potential data leaks.

💡 **Inspect network traffic inside a container using** tcpdump:

```sh
docker exec -it <container_id> tcpdump -i eth0
```

---

**5. BLOCK I/O (Disk Read/Write Operations)**

**•	First value:** Data read from disk.

**•	Second value:** Data written to disk.

**Interpreting Disk I/O Levels**

✅ **<10MB** → Expected for lightweight apps
⚠️ **10MB - 500MB** → Moderate, typical for logging or caching
🚨 **500MB - 5GB** → High, likely due to frequent file operations or active databases
🔥 **>5GB → Excessive!** May indicate inefficient logging, batch jobs, or heavy writes

💡 **Optimizing High Disk I/O:**

**•	Reduce excessive logging:**

```sh
docker run --log-opt max-size=10m --log-opt max-file=3 my_container
```

**•	Optimize database-heavy applications:**

o	Implement **caching** (e.g., Redis).

o	Optimize **query performance.**

---

**6. PIDS (Number of Active Processes in the Container)**

•	Shows the **number of running processes inside the container.**

**Interpreting Process Counts**

✅ **1-10** → Simple applications
⚠️ **10-50** → Moderate complexity
🚨 **>50** → Possible process leaks

💡 **Limit the number of processes in a container to prevent resource exhaustion:**

```sh
docker run --pids-limit=50 your_container
```

---

**Monitoring Specific Containers**

By default, docker stats shows resource usage for **all running containers.**

To monitor a **specific container:**

```sh
docker stats <container_id_or_name>
```

To filter containers by **name pattern:**

```sh
docker stats --filter "name=techops-*"
```

**Use Cases: Real-World Scenarios**

**1. Troubleshooting High Resource Utilization**

When a container is consuming high CPU or memory, use docker stats to identify the issue and take corrective actions like scaling resources or restarting the container.

**2. Capacity Planning**

By monitoring docker stats, DevOps teams can determine if resources are over- or under-utilized and adjust configurations accordingly.

**3. Load Testing and Benchmarking**

Use docker stats to analyze container behavior under load tests to ensure optimal performance before deploying to production.

**4. Identifying Memory Leaks**

If memory usage continuously increases over time, docker stats can help identify memory leaks in applications running inside containers.

**5. Ensuring Network Stability**

Monitor network traffic to ensure containers communicate efficiently, and detect unusual spikes that may indicate security threats.

---

**Automating Monitoring**

To automate monitoring, use a simple Bash script to log container statistics.

**Example: Log** docker stats **Output to a File**

```sh
#!/bin/bash
while true; do
    docker stats --no-stream >> /var/log/docker_stats.log
    sleep 60  # Capture stats every 60 seconds
done
```

Run the script in the background:

```sh
nohup ./monitor_docker_stats.sh &
```

**Integrating with Prometheus and Grafana**

For advanced monitoring, consider using **Prometheus** to scrape container metrics and visualize them using **Grafana.**

•	Install cAdvisor:

```sh
docker run -d --name=cadvisor \
  --volume=/:/rootfs:ro \
  --volume=/var/run:/var/run:ro \
  --volume=/sys:/sys:ro \
  --volume=/var/lib/docker/:/var/lib/docker:ro \
  --publish=8080:8080 \
  google/cadvisor:latest
```

•	Configure Prometheus to scrape metrics from cAdvisor.

---

**Comparison with Other Monitoring Tools**

| **Tool**             | **Features** |
|----------------------|-------------|
| **`docker stats`**   | Basic real-time monitoring for **CPU, memory, network, and disk usage**. |
| **cAdvisor**         | Detailed container monitoring with **historical metrics, storage tracking, and per-process stats**. |
| **Prometheus + Grafana** | Advanced monitoring with **time-series data, visualization, and alerting** for containerized environments. |
| **ELK Stack (Elasticsearch, Logstash, Kibana)** | Centralized **logging and monitoring** with powerful search and visualization capabilities. |

For production environments, docker stats is a great starting point, but tools like **Prometheus, Grafana, and ELK** provide better insights and historical data analysis.

---

**Conclusion**

docker stats is a powerful built-in tool for real-time container resource monitoring. It provides essential metrics to troubleshoot performance issues, optimize resource allocation, and maintain containerized applications.

**Best Practices:**

•	Use docker stats regularly to monitor container health.
•	Automate monitoring using scripts or external tools like Prometheus.
•	Combine docker stats with cAdvisor, Prometheus, or ELK for detailed analysis.
•	Set resource limits (--memory, --cpus) to prevent resource contention.

By leveraging **docker stats** efficiently, DevOps engineers can ensure smooth operation of containerized applications.
