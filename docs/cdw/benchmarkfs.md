---
layout: default
title: Demystifying Data Storage Format in CDW
parent: Cloudera Data Warehouse
---

# Parquet, Orc, Avro and CSV Benchmarking
{: .no_toc }

CDW is 

- TOC
{:toc}

---


## Hardware

- The performance benchmarking tests are carried out using the following hardware specification.

| CPU          | Intel(R) Xeon(R) Gold 5220R CPU @ 2.20GHz | 
| Memory  | DIMM DDR4 Synchronous Registered (Buffered) 2933 MHz (0.3 ns) | 
| Disk | SSD P4610 1.6TB SFF    | 

- The datasheet for the storage disk `SSD P4610` can be obtained [here](https://ark.intel.com/content/www/us/en/ark/products/140103/intel-ssd-dc-p4610-series-1-6tb-2-5in-pcie-3-1-x4-3d2-tlc.html). The performance specifications stated in this datasheet can be used to benchmark against the result of the tests.

## Architecture

- The Longhorn performance tests are carried out in a Kubernetes cluster with 3 physical nodes connected to each other on the same subnet. Each test run inside the stateful pod that is attached to the storage class backed by the Longhorn persistent volume with replica size 2 and 3.
- The local attached storage performance test is carried out directly on the operating system of the physical server.

![](../../assets/images/longhorn/bench5.png) 

## Storage Performance Tool

- `fio` is used as the storage performance tool using different block size (bs) and IOdepth as illustrated in the following table. Part of the tests are performed based on random 50% read and 50% write (files scattered around the disk). The rest is based on sequential 100% read and 100% write operations.

| Block Size (bs)      | IOdepth         |
|:-------------|:------------------|
| 4k          | 32        | 
| 8k        | 32         | 
| 64k       | 16           | 
| 1024k     | 8          | 


- The full `fio` command with random 50% read and 50% write operation is shown as follows.

```yaml
fio --name=fiotest --filename=test --size=10Gb --numjobs=8 --ioengine=libaio --group_reporting --runtime=60 --startdelay=60 --bs=8k --iodepth=32 --rw=randrw --direct=1 --rwmixread=50
```

