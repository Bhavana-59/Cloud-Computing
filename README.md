

# Experiment 1 — Performance Analysis of Type-1 and Type-2 Hypervisors

## Objective

To create identically configured Ubuntu virtual machines on a **Type-1 hypervisor (Proxmox VE)** and a **Type-2 hypervisor (VMware Workstation)** and compare their CPU performance using **Sysbench**.

The performance comparison is based on:

- Total execution time
- Total events
- Events per second
- Average latency
- CPU and memory resource utilization

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
````

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

The same guest operating system, virtual CPU, memory, storage, and Sysbench workload are used as specified by the experiment manual so that the performance results can be compared.

---

## 2. Standard VM Configuration

Both virtual machines use the following configuration:

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

* Total execution time
* Total number of events
* Events per second
* Average latency

## 3.5 Type-1 Results

> **Status:** Completed — Sysbench benchmark result collected from the Proxmox VM.

| Metric               | Type-1 — Proxmox |
| -------------------- | ---------------: |
| Total execution time |        10.0004 s |
| Total events         |           17,257 |
| Events per second    |         1,725.49 |
| Average latency      |          0.58 ms |

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

> **Status:** Completed — Sysbench benchmark result collected from the VMware VM.

| Metric               | Type-2 — VMware |
| -------------------- | --------------: |
| Total execution time |       10.0010 s |
| Total events         |          10,274 |
| Events per second    |        1,027.00 |
| Average latency      |         0.97 ms |

---

# 5. Performance Comparison

The measured Sysbench results are compared below.

| Metric               | Type-1 — Proxmox | Type-2 — VMware |
| -------------------- | ---------------: | --------------: |
| Total execution time |        10.0004 s |       10.0010 s |
| Total events         |           17,257 |          10,274 |
| Events per second    |         1,725.49 |        1,027.00 |
| Average latency      |          0.58 ms |         0.97 ms |

---

# 6. Performance Analysis

The Sysbench CPU benchmark was executed on both virtual machines using the same workload:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

The recorded results show differences in the benchmark metrics between the Type-1 Proxmox environment and the Type-2 VMware environment.

### Execution Time

The Proxmox VM recorded a total execution time of **10.0004 seconds**, while the VMware VM recorded **10.0010 seconds**.

### Events per Second

The Proxmox VM recorded **1,725.49 events/sec**, while the VMware VM recorded **1,027.00 events/sec**.

### Latency

The average latency recorded was **0.58 ms** for Proxmox and **0.97 ms** for VMware.

### Resource Utilization

CPU and memory resource utilization are observed during the experiment to understand resource usage.

For Proxmox, resource utilization can be observed through the VM **Summary** page. VMware resource usage can be observed through the virtual machine and host environment during benchmark execution.

The benchmark results provide the measured performance values for the two hypervisor environments under the configured experimental conditions.

---

# 7. Repository Structure

```text
CC-LAB/
└── exp-1/
    ├── README.md
    │
    ├── screenshots/
    │   ├── type1-proxmox/
    │   │   ├── 01-proxmox-dashboard.png
    │   │   ├── 02-proxmox-vm-configuration.png
    │   │   ├── 03-proxmox-vm-running.png
    │   │   ├── 04-proxmox-ubuntu-console.png
    │   │   ├── 05-proxmox-system-configuration.png
    │   │   └── 06-proxmox-sysbench-result.png
    │   │
    │   ├── type2-vmware/
    │   │   ├── 01-vmware-vm-configuration.png
    │   │   └── 04-vmware-sysbench-result.png
    │   │
    │   └── comparison/
    │
    └── results/
```

---

# 8. Tools Used

* Proxmox VE
* VMware Workstation
* Ubuntu
* Sysbench
* Git
* GitHub

---

# 9. Benchmark Command

The CPU benchmark used for both hypervisors is:

```bash
sysbench cpu --cpu-max-prime=20000 run
```

The same workload is used for both environments to support a consistent performance comparison.

---

## Conclusion

This experiment studies the performance of virtual machines running on Type-1 and Type-2 hypervisors.

The Type-1 environment uses **Proxmox VE**, while the Type-2 environment uses **VMware Workstation**. Both environments use an Ubuntu guest operating system with the specified virtual resources and the same Sysbench CPU workload.

The measured Sysbench results provide a basis for comparing CPU execution time, event throughput, and latency under the configured experimental conditions.




