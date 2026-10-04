
# Experiment 2 — Performance Analysis of Virtual Machines and Containers

## 1. Aim

To compare the performance of a Virtual Machine (VM) and a Docker container by evaluating CPU, memory, disk I/O, network performance, and application-level execution under a controlled experimental environment.

---

## 2. Objectives

- Understand the difference between Virtual Machine and container-based virtualization.
- Configure and verify the Virtual Machine environment.
- Configure and verify the Docker container environment.
- Measure CPU performance using benchmarking tools.
- Compare memory and disk I/O performance.
- Measure and compare network performance.
- Run a containerized FastAPI application and verify application-level execution.
- Analyze the collected results and observations.

---

## 3. Introduction

Virtual Machines and containers are two commonly used approaches for virtualization in cloud computing.

A **Virtual Machine** runs a complete guest operating system on virtualized hardware managed by a hypervisor. This provides strong isolation but requires a separate operating system environment for each VM.

A **Docker container** provides application-level isolation while sharing the host operating system kernel. Containers are generally lightweight and can be created and started quickly.

In this experiment, both environments are evaluated using system-level and application-level performance measurements.

---

## 4. VM vs Container

| Feature | Virtual Machine | Docker Container |
|---|---|---|
| Virtualization level | Hardware / OS level | Application level |
| Operating system | Complete guest OS | Shares host kernel |
| Isolation | Strong | Process-level isolation |
| Resource overhead | Higher | Lower |
| Startup time | Relatively higher | Relatively lower |
| Management | Hypervisor | Docker |
| Experiment focus | System performance | Container performance |

---

## 5. Experimental Environment

### 5.1 Virtual Machine

| Parameter | Configuration |
|---|---|
| Platform | VMware Virtual Platform |
| Operating System | Ubuntu 24.04.5 LTS |
| CPU | 2 virtual CPUs |
| Memory | Approximately 1.9 GiB |
| Virtual Disk | 20 GB |
| Swap | Approximately 3.4 GiB |

### 5.2 Docker Container

| Parameter | Configuration |
|---|---|
| Container Runtime | Docker |
| Docker Version | 29.1.3 |
| Base Image | Ubuntu 24.04 |
| CPU Limit | 1 CPU |
| Memory Limit | 512 MB |
| Memory cgroup limit | 536870912 bytes |
| CPU cgroup configuration | 100000 / 100000 |

### 5.3 Tools Used

| Tool | Purpose |
|---|---|
| Sysbench | CPU benchmarking |
| fio | Disk I/O benchmarking |
| iperf3 | Network performance measurement |
| Docker | Container creation and management |
| FastAPI | Application-level testing |
| Python | Data processing and graph generation |

---

## 6. Experiment Architecture

```text
                    VM vs Container
                          |
             +------------+------------+
             |                         |
       Virtual Machine           Docker Container
             |                         |
        Ubuntu OS                 Ubuntu Image
             |                         |
      +------+------+          +------+------+
      |      |      |          |      |      |
     CPU   Memory  Disk        CPU   Memory  Disk
      |      |      |          |      |      |
      +------+------+          +------+------+
             |                         |
             +------------+------------+
                          |
                    Network Testing
                          |
                        iperf3
                          |
                 Performance Results
````

### FastAPI Application Architecture

```text
              Client
                 |
                 v
       Containerized FastAPI
                 |
          +------+------+
          |             |
          v             v
      Health API    Compute API
          |             |
          v             v
     Health Status   Computation
       Response        Result
```

---

## 7. Experiment Workflow

```text
System Setup
     |
     v
VM Configuration
     |
     v
Docker Configuration
     |
     v
Container Creation
     |
     v
CPU Benchmark
     |
     v
Memory Benchmark
     |
     v
Disk I/O Benchmark
     |
     v
Network Benchmark
     |
     v
FastAPI Application Test
     |
     v
Result Collection
     |
     v
Performance Analysis
     |
     v
Conclusion
```

---

## 8. Experimental Procedure

### Step 1 — Verify VM Environment

The VM environment was first verified using Linux system-information commands.

The following parameters were checked:

* CPU configuration
* Memory
* Storage
* Operating system
* Kernel
* Virtualization platform

The corresponding evidence is available in the `screenshots/` directory.

---

### Step 2 — Verify Docker Environment

Docker installation and configuration were verified before performing the container experiments.

The following were checked:

* Docker installation
* Docker version
* Container configuration
* Resource limits
* Required benchmarking tools

---

### Step 3 — Create and Verify Container

A Docker container was created using the configured Ubuntu-based environment.

The container was verified to ensure that:

* The container starts successfully.
* Required tools are available.
* Resource limits are applied.
* Benchmarking commands can be executed.

---

### Step 4 — CPU Performance Test

CPU performance was evaluated using **Sysbench**.

The benchmark was performed for the VM and container environments, and the resulting measurements were collected for comparison.

---

### Step 5 — Memory Performance Test

Memory performance was evaluated using the configured container environment.

The container was restricted to approximately **512 MB memory** to demonstrate controlled resource allocation using Docker and cgroups.

---

### Step 6 — Disk I/O Performance Test

Disk performance was evaluated using **fio**.

The following operations were tested:

* Sequential Read
* Sequential Write
* Random Read
* Random Write

The VM and container disk results were collected for comparison.

---

### Step 7 — Network Performance Test

Network performance was evaluated using **iperf3**.

The VM and container network environments were tested separately and their measured throughput values were recorded.

---

### Step 8 — FastAPI Application Test

A FastAPI application was used for application-level testing inside the container.

The application provides endpoints for:

* Health checking
* Computation

The following were verified:

1. FastAPI container is running.
2. Health endpoint responds successfully.
3. Compute endpoint executes successfully.
4. Containerized application can be accessed through its exposed endpoints.

---

## 9. Benchmark Categories

The experiment evaluates the following performance parameters:

| Category    | Tool / Method    |
| ----------- | ---------------- |
| CPU         | Sysbench         |
| Memory      | Memory benchmark |
| Disk I/O    | fio              |
| Network     | iperf3           |
| Application | FastAPI          |

---

## 10. Results

The experiment collects performance measurements for the following parameters:

| Parameter             | VM       | Container |
| --------------------- | -------- | --------- |
| CPU                   | Measured | Measured  |
| Memory                | Measured | Measured  |
| Sequential Disk Read  | Measured | Measured  |
| Sequential Disk Write | Measured | Measured  |
| Random Disk Read      | Measured | Measured  |
| Random Disk Write     | Measured | Measured  |
| Network               | Measured | Measured  |
| Application Execution | Verified | Verified  |

> **Note:** Exact numerical benchmark values are maintained in `results/processed/benchmark_results.csv`.

---

## 11. Performance Graphs

Generated performance graphs are stored in the `results/figures/` directory.

### CPU Comparison

```text
results/figures/01-cpu-comparison.png
```

### Network Comparison

```text
results/figures/02-network-comparison.png
```

### Container Memory Performance

```text
results/figures/03-container-memory-performance.png
```

### Processed Benchmark Data

```text
results/processed/benchmark_results.csv
```

### Graph Generation Script

```text
results/processed/generate_graphs.py
```

---

## 12. FastAPI Application Testing

The FastAPI application was tested inside the Docker container.

| Test             | Purpose                                |
| ---------------- | -------------------------------------- |
| Health Endpoint  | Verify service availability            |
| Compute Endpoint | Verify application computation         |
| Container Status | Verify application container execution |
| API Endpoints    | Verify application accessibility       |

The corresponding screenshots are available in the `screenshots/` directory.

---

## 13. Observations

The following observations were made during the experiment:

1. Both VMs and containers provide isolated execution environments.

2. A VM provides a complete guest operating system environment.

3. Containers share the host operating system kernel.

4. Docker allows CPU and memory resources to be controlled.

5. CPU performance can be evaluated using Sysbench.

6. Disk I/O performance can be evaluated using fio.

7. Network throughput can be measured using iperf3.

8. Containerized applications can be deployed and accessed through application endpoints.

9. The collected benchmark results provide a practical basis for comparing VM and container execution.

---

## 14. VM and Container Comparison

| Aspect                 | Virtual Machine         | Docker Container                    |
| ---------------------- | ----------------------- | ----------------------------------- |
| Isolation              | Complete guest OS       | Application/process isolation       |
| Kernel                 | Own guest kernel        | Shares host kernel                  |
| Resource overhead      | Higher                  | Lower                               |
| Startup                | Relatively slower       | Relatively faster                   |
| Resource control       | VM allocation           | Docker limits / cgroups             |
| Application deployment | Complete OS environment | Lightweight application environment |
| CPU Testing            | Sysbench                | Sysbench                            |
| Disk Testing           | fio                     | fio                                 |
| Network Testing        | iperf3                  | iperf3                              |
| Application Testing    | FastAPI                 | Containerized FastAPI               |

---

## 15. Experimental Evidence

All screenshots are stored under:

```text
screenshots/
```

### System and Configuration

```text
01-docker-hello-world.png
02-lscpu-system-info - Copy.png
03-memory-and-storage-info - Copy.png
04-disk-and-docker-info.png
05-vm-configuration.png
06-docker-configuration.png
```

### Benchmark Setup

```text
07-baseline-cpu-sysbench.png
08-benchmark-dockerfile.png
09-benchmark-docker-image - Copy.png
10-container-tools-verification - Copy.png
```

### CPU and Memory

```text
11-vm-cpu-result.png
12-container-cpu-result.png
13-container-memory-result.png
```

### Disk

```text
14-vm-disk-results.png
15-container-disk-results.png
```

### Network

```text
16-vm-network-iperf3.png
17-container-network-iperf3.png
```

### FastAPI Application

```text
18-fastapi-health.png
19-fastapi-compute.png
20-fastapi-container-running.png
21-container-fastapi-endpoints.png
```

---

## 16. Repository Structure

```text
Experiment 2 — VM vs Container Performance Analysis/
│
├── README.md
│
├── api/
│   ├── Dockerfile
│   ├── main.py
│   └── requirements.txt
│
├── docker/
│   └── Dockerfile
│
├── docs/
│   ├── cpu-info.txt
│   ├── kernel-info.txt
│   ├── memory-info.txt
│   └── storage-info.txt
│
├── results/
│   ├── figures/
│   │   ├── 01-cpu-comparison.png
│   │   ├── 02-network-comparison.png
│   │   └── 03-container-memory-performance.png
│   │
│   └── processed/
│       ├── benchmark_results.csv
│       └── generate_graphs.py
│
└── screenshots/
    ├── 01-docker-hello-world.png
    ├── 02-lscpu-system-info - Copy.png
    ├── 03-memory-and-storage-info - Copy.png
    ├── 04-disk-and-docker-info.png
    ├── 05-vm-configuration.png
    ├── 06-docker-configuration.png
    ├── 07-baseline-cpu-sysbench.png
    ├── 08-benchmark-dockerfile.png
    ├── 09-benchmark-docker-image - Copy.png
    ├── 10-container-tools-verification - Copy.png
    ├── 11-vm-cpu-result.png
    ├── 12-container-cpu-result.png
    ├── 13-container-memory-result.png
    ├── 14-vm-disk-results.png
    ├── 15-container-disk-results.png
    ├── 16-vm-network-iperf3.png
    ├── 17-container-network-iperf3.png
    ├── 18-fastapi-health.png
    ├── 19-fastapi-compute.png
    ├── 20-fastapi-container-running.png
    └── 21-container-fastapi-endpoints.png
```

---

## 17. Conclusion

This experiment provides a practical comparison between Virtual Machines and Docker containers using system-level and application-level performance measurements.

CPU performance was evaluated using Sysbench, disk I/O using fio, and network performance using iperf3. Docker resource limits were also used to demonstrate controlled resource allocation for containers.

A FastAPI application was additionally deployed inside a Docker container and tested through its application endpoints.

The experiment demonstrates the differences between VM-based and container-based virtualization and provides practical experience with performance benchmarking, container resource management, and application deployment.

---

## 18. Key Learning Outcomes

After completing this experiment, we understand:

* The difference between Virtual Machines and containers.
* How to configure and verify a VM environment.
* How to create and manage Docker containers.
* How to perform CPU benchmarking using Sysbench.
* How to apply memory limits to containers.
* How to measure disk I/O using fio.
* How to measure network throughput using iperf3.
* How to deploy and test a FastAPI application inside a container.
* How to collect and process benchmark results.
* How to generate performance graphs.
* How to compare VM and container performance experimentally.

---

## 19. Final Deliverables

The experiment includes:

* VM configuration evidence
* Docker configuration evidence
* Container setup and verification
* CPU benchmark results
* Memory performance results
* Disk I/O results
* Network performance results
* FastAPI application testing
* Experimental screenshots
* Processed benchmark data
* Performance graphs
* Performance observations
* Final conclusion

---
**Name:** *Bhavana*
**Course:** Cloud Computing
**Experiment:** 2 — Performance Analysis of Virtual Machines and Containers
**Academic Year:** 2026–27
