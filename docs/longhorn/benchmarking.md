---
layout: default
title: Performance Benchmarking
parent: Longhorn
---

# Performance Benchmarking
{: .no_toc }

Longhorn is the cloud native distributed block storage in the Kubernetes ecosystem. It is a software defined storage that synchronously replicates the volumes across multiple nodes to achieve high availability and high degree of resiliency. Replicas should be hosted on separate nodes to avoid single point of failure. As a result, Longhorn volume is used as the persistent volume for the stateful pods in Kubernetes. 

This article describes the performance output with the following benchmarking. The objective is to gauge the performance output in terms of IOPS, latency, bandwidth and CPU usage when using Longhorn system with specific hardware model.

- Longhorn storage vs local attached disk
- Longhorn replica size 2 vs Longhorn replica size 3 

The performance benchmarking tests had been carried out using the following hardware specification.

| CPU          | Intel(R) Xeon(R) Gold 5220R CPU @ 2.20GHz | 
| Memory  | DIMM DDR4 Synchronous Registered (Buffered) 2933 MHz (0.3 ns) | 
| Disk | NVMe P4610 1.6TB SFF    | 

---

1. The tests were carried out by running the `fio` tool using different block size and IOdepth as illustrated in the following table. Part of the tests were performed based on random Read/Write and part of them were based 100% Read/Write.

| Block Size       | IOdepth         |
|:-------------|:------------------|
| 4k          | 32        | 
| 8k        | 32         | 
| 64k       | 16           | 
| 1024k     | 8          | 


2. Check the status of the direct attached disks in this host.

    ![](../../assets/longhorn/bench1.png) 

