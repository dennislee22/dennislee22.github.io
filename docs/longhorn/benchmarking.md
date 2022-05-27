---
layout: default
title: Performance Benchmarking
parent: Longhorn
---

# Performance Benchmarking
{: .no_toc }

Longhorn is the cloud native distributed block storage in the Kubernetes ecosystem. It is a software defined storage that synchronously replicates the volumes across multiple nodes to achieve high availability and high degree of resiliency. This article describes its performance output in comparison to the local attached storage. The performance benchmarking tests were carried out using the following hardware specification.


| CPU          | Intel(R) Xeon(R) Gold 5220R CPU @ 2.20GHz | 
| Memory  | DIMM DDR4 Synchronous Registered (Buffered) 2933 MHz (0.3 ns) | 
| Disk | NVMe P4610 1.6TB SFF    | 

---

1. T

    ```bash
    # yum install lvm2 -y
    ```

2. Check the status of the direct attached disks in this host.

    ```bash
    # lsblk
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sr0     11:0    1  368K  0 rom  
    vda    253:0    0   99G  0 disk 
    `-vda1 253:1    0   99G  0 part /
    vdb    253:16   0  500G  0 disk 
    vdc    253:32   0  500G  0 disk 
    vdd    253:48   0  400G  0 disk 
    ```

