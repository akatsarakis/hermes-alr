# Hermes-ALR

This repository contains Hermes—a membership-based replication protocol ensuring linearizability under partial synchrony—enhanced with (Eager) Almost Local Reads (ALRs). This enhancement removes Hermes’ reliance on time-based leases, guaranteeing linearizable local reads at each replica, even in asynchronous environments.

## Overview

* **Baseline Protocol**: Hermes provides crash-tolerance, linearizable operations, and efficient local reads at each replica.
* **Hermes-ALR**: Integrates Eager-ALRs (also known as `async_reads` in the code), eliminating extra timing assumptions and ensuring linearizable local reads without leases.

## Key Features

* **Crash-Tolerant, Linearizable**: Guarantees correctness despite replica failures.
* **Almost-local Linearizable Reads**: Each replica can serve high throughput reads locally without incurring overhead to other replicas.
* **Lease-Free**: Removes the risk of time-based lease violations under timing failures.

## Compilation

By default, we compile Hermes compiles without ALRs. To enable ALRs:

```c
#define ENABLE_ASYNC_ONLY_READ
```
in include/hermes/config.h file
> **Note:** In the Hermes-ALR branch, ALRs are usually referred to as `async_reads`.

# CloudLab Environment Setup

To replicate the experimental setup described in the paper:

1. **Use the following CloudLab profile**:  
   [CloudLab RDMA cluster profile](https://www.cloudlab.us/p/LawTheorem/rdma-cluster-img)
2. **Instantiate** 3–7 `r320` servers with RDMA interconnects.
3. **Finalize** and bring up all servers.

Once the cluster is ready:

- Copy the hostnames into `bin/init_cloudlab.sh` **in the order they appear**.
- Update your CloudLab username, SSH config path, and CloudLab SSH key path in the same script.

---

## Running Hermes(-ALR)

1. **SSH** into the first node (e.g., `ssh node1`).
2. **Compile Hermes(-ALR)**:
   ```bash
   # From the top-level directory
   make clean && make

# Running Hermes(-ALR)

Use **one** of the following methods:

1. **Original Hermes instructions**  
2. **Helper scripts** (e.g., `bin/copy-n-exec-hermesKV.sh`) after updating IPs in `exec/hosts.sh`.

---

## Lazy ALRs in Odyssey

(Lazy) ALRs (referred to as _bqrs_ in Odyssey) are integrated directly into the original [Odyssey repository](https://github.com/vasigavr1/Odyssey). For protocols such as **ZAB** and **Raft**:

1. Use the **same CloudLab setup** as above.  
2. To enable or disable ALRs, add the following definitions in the appropriate header files (i.e., `Zookeeper/include/zookeeper/zk_config.h`):
```c
#define ENABLE_ALRS       // or
#define ZK_ENABLE_BQR
```
3. Follow the **Odyssey instructions** to run experiments.

## Contact
 Antonios Katsarakis: [`antoniskatsarakis@yahoo.com`](mailto:antoniskatsarakis@yahoo.com?subject=[GitHub]%20Hermes%repo)
