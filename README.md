# Experiment 1 — Performance Analysis of Type-1 and Type-2 Hypervisors

## Objective

To create identically configured Ubuntu virtual machines on a **Type-1 hypervisor (Proxmox VE)** and a **Type-2 hypervisor (VMware Workstation)** and compare their CPU performance using **Sysbench**.

The performance comparison is based on:

* Total execution time
* Total events
* Events per second
* Average latency
* CPU and memory resource utilization

---

## 1. Experiment Overview

A hypervisor is a software layer that creates and manages virtual machines and allocates physical system resources such as CPU, memory, storage, and networking to them.

This experiment compares two types of hypervisors:

### Type-1 Hypervisor

A Type-1 hypervisor runs directly on the physical hardware.

For this experiment:

**Proxmox VE** is used as the Type-1 hypervisor.

```text
Physical Hardware
       ↓
   Proxmox VE
       ↓
    Ubuntu VM
       ↓
    Sysbench
```

### Type-2 Hypervisor

A Type-2 hypervisor runs on top of a host operating system.

For this experiment:

**VMware Workstation** is used as the Type-2 hypervisor.

```text
Physical Hardware
       ↓
     Windows
       ↓
VMware Workstation
       ↓
    Ubuntu VM
       ↓
    Sysbench
```

The same guest operating system, virtual CPU, memory, storage, and Sysbench workload are used as far as specified by the experiment manual so that the performance results can be compared.

---

## 2. Standard VM Configuration

Both virtual machines should use the following configuration:

| Resource      | Configuration                            |
| ------------- | ---------------------------------------- |
| Guest OS      | Ubuntu 22.04 or later                    |
| vCPU          | 2 vCPU                                   |
| RAM           | 2 GB / 2048 MiB                          |
| Disk          | 20 GB                                    |
| Benchmark     | Sysbench CPU                             |
| CPU Benchmark | `sysbench cpu --cpu-max-prime=20000 run` |

---

# 3. Type-1 Hypervisor — Proxmox VE

## 3.1 Proxmox Configuration

The Type-1 virtual machine is created using Proxmox VE.

| Setting    | Configuration        |
| ---------- | -------------------- |
| Hypervisor | Proxmox VE           |
| VM Name    | CC-Experiment1-Type1 |
| Guest OS   | Ubuntu               |
| CPU        | 1 socket × 2 cores   |
| Total vCPU | 2                    |
| Memory     | 2048 MiB             |
| Disk       | 20 GB                |
| Network    | `vmbr0`              |

The Proxmox VM is accessed through the Proxmox web interface:

```text
https://<PROXMOX_SERVER_IP>:8006
```

## 3.2 System Verification

Inside the Ubuntu VM, the following commands are used to verify the system configuration:

```bash
hostnamectl
lscpu
free -h
df -h
top
```

## 3.3 Install Sysbench

```bash
sudo apt update
sudo apt install sysbench -y
```

Verify the installation:

```bash
sysbench --version
```

## 3.4 CPU Benchmark

Run:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

The following values are recorded:

* Total time
* Total number of events
* Events per second
* Average latency

## 3.5 Type-1 Results

> **Status:** Pending — Proxmox system will be accessed when the college/lab environment is available.

| Metric               | Type-1 — Proxmox |
| -------------------- | ---------------: |
| Total execution time |         10.004s  |
| Total events         |           17257  |
| Events per second    |          1725.49 |
| Average latency      |             0.58 |

## 3.6 Resource Monitoring

Proxmox resource utilization can be observed from the VM's **Summary** page.

The following resources are monitored:

* CPU utilization
* Memory utilization
* Network activity
* Disk activity

---

# 4. Type-2 Hypervisor — VMware Workstation

## 4.1 VMware Configuration

The Type-2 virtual machine is created using VMware Workstation.

| Setting    | Configuration         |
| ---------- | --------------------- |
| Hypervisor | VMware Workstation    |
| Guest OS   | Ubuntu                |
| CPU        | 1 processor × 2 cores |
| Total vCPU | 2                     |
| Memory     | 2048 MB               |
| Disk       | 20 GB                 |
| Network    | NAT                   |

The Ubuntu VM runs on top of the host operating system through VMware Workstation.

## 4.2 System Verification

Inside the Ubuntu VM:

```bash
hostnamectl
lscpu
free -h
df -h
top
```

These commands are used to verify the guest operating system and allocated resources.

## 4.3 Install Sysbench

```bash
sudo apt update
sudo apt install sysbench -y
```

Verify:

```bash
sysbench --version
```

## 4.4 CPU Benchmark

Run the same benchmark used for the Type-1 hypervisor:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

Record:

* Total execution time
* Total events
* Events per second
* Average latency

## 4.5 Type-2 Results

The actual values will be filled after running the benchmark.

| Metric               | Type-2 — VMware |
| -------------------- | --------------: |
| Total execution time |        10.0010s |
| Total events         |           10274 |
| Events per second    |         1027.00 |
| Average latency      |            0.97 |

---

# 5. Performance Comparison

After both experiments are completed, the measured values will be compared.

| Metric               | Type-1 — Proxmox | Type-2 — VMware |
| -------------------- | ---------------: | --------------: |
| Total execution time |          10.004s |        10.0010s |
| Total events         |            17257 |            1024 |
| Events per second    |          1725.49 |         1027.00 |
| Average latency      |              0.58|            0.97 |

The comparison will be based only on the actual measurements obtained from the two environments.

---

# 6. Performance Analysis

The experiment evaluates how the two hypervisor types perform under the same CPU benchmark workload.

The following aspects will be analyzed:

### Execution Time

The total time required to complete the Sysbench CPU workload.

### Events per Second

The number of benchmark events completed per second.

### Latency

The time taken to process individual benchmark events.

### Resource Utilization

CPU and memory utilization are observed during the experiment to understand resource usage.

The final analysis will be added after collecting both Type-1 and Type-2 results.

---

# 7. Screenshots

Screenshots are organized according to the experiment manual.

```text
screenshots/
├── type1-proxmox/
│   ├── 01-proxmox-dashboard.png
│   ├── 02-proxmox-vm-configuration.png
│   ├── 03-proxmox-vm-running.png
│   ├── 04-proxmox-ubuntu-console.png
│   ├── 05-proxmox-system-configuration.png
│   └── 06-proxmox-sysbench-result.png
│  
│
├── type2-vmware/
│   ├── 01-vmware-vm-configuration.png
│   ├── 02-vmware-vm-running.png
│   ├── 03-vmware-system-configuration.png
│   └── 04-vmware-sysbench-result.png
│
└── comparison/
    └── 01-hypervisor-performance-comparison.png
```

---

# 8. Results

Detailed performance observations and analysis will be maintained in:

```text
results/performance-analysis.md
```

The final comparison will be completed after both Type-1 and Type-2 benchmark results are available.

---

# 9. Repository Structure

```text
CC-LAB/
└── exp-1/
    ├── README.md
    │
    ├── screenshots/
    │   ├── type1-proxmox/
    │   ├── type2-vmware/
    │   └── comparison/
    │
    └── results/
        └── performance-analysis.md
```

---

# 10. Tools Used

* Proxmox VE
* VMware Workstation
* Ubuntu
* Sysbench
* Git
* GitHub

---

# 11. Benchmark Command

The CPU benchmark used for both hypervisors is:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

The same workload is used for both environments to support a consistent performance comparison.

---

## Conclusion

This experiment studies the performance of virtual machines running on Type-1 and Type-2 hypervisors.

The Type-1 environment uses **Proxmox VE**, while the Type-2 environment uses **VMware Workstation**. Both environments use an Ubuntu guest operating system with the specified virtual resources and the same Sysbench CPU workload.

The final performance comparison and conclusions will be added after collecting measurements from both environments.
