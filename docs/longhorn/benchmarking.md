---
layout: default
title: Performance Benchmarking
parent: Longhorn
---

# Longhorn Performance Benchmarking
{: .no_toc }

Longhorn is the cloud native distributed block storage in the Kubernetes ecosystem. It is a software defined storage that synchronously replicates the volumes across multiple Kubernetes nodes to achieve high availability and high degree of resiliency. Longhorn volume is used as the persistent volume for the stateful pods in Kubernetes. Longhorn volume replicas should be hosted on separate nodes to prevent single point of failure.

This article describes the performance output as the result of using `fio` tool with the following benchmarking agenda. The objective is to gauge the performance output in terms of IOPS, latency, bandwidth and CPU usage when using Longhorn storage with specific hardware model.

- Longhorn storage vs local attached storage
- Longhorn replica size 2 vs replica size 3 

---

## Hardware

The performance benchmarking tests were carried out using the following hardware specification.

| CPU          | Intel(R) Xeon(R) Gold 5220R CPU @ 2.20GHz | 
| Memory  | DIMM DDR4 Synchronous Registered (Buffered) 2933 MHz (0.3 ns) | 
| Disk | NVMe P4610 1.6TB SFF    | 

The datasheet for `NVMe P4610` disk can be obtained [here](https://ark.intel.com/content/www/us/en/ark/products/140103/intel-ssd-dc-p4610-series-1-6tb-2-5in-pcie-3-1-x4-3d2-tlc.html). The performance specifications mentioned in this datasheet can be used to benchmark against the result of the tests.

## Architecture

The tests were carried out in a Kubernetes cluster with 3 physical nodes connected to each other on the same subnet.

## Storage Performance Tool

The tests were carried out by running the `fio` tool using different block size and IOdepth as illustrated in the following table. Part of the tests were performed based on random Read/Write and the rest were based on 100% Read/Write operation.

| Block Size       | IOdepth         |
|:-------------|:------------------|
| 4k          | 32        | 
| 8k        | 32         | 
| 64k       | 16           | 
| 1024k     | 8          | 


The full `fio` command with random 50% read and 50% write operation is shown as follows.

```yaml
fio --name=fiotest --filename=test --size=10Gb --numjobs=8 --ioengine=libaio --group_reporting --runtime=60 --startdelay=60 --bs=8k --iodepth=32 --rw=randrw --direct=1 --rwmixread=50
```

The full `fio` command for write only is shown as follows.

```yaml
fio --name=fiotest --filename=test --size=10Gb --direct=1 --numjobs=8 --ioengine=libaio --group_reporting --runtime=60 --startdelay=60 --bs=8k --iodepth=32 --rw=write
```

The abovementioned commands were run inside the Kubernetes pod as illustrated in the following example.

```bash
# kubectl exec -ti fio-0 -n test -- /bin/bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
[root@fio-0 /]# fio --name=fiotest --filename=test --size=10Gb --rw=randrw --direct=1 --rwmixread=50 --numjobs=8 --ioengine=libaio --group_reporting --runtime=60 --startdelay=60 --bs=1M --iodepth=8
fiotest: (g=0): rw=randrw, bs=(R) 1024KiB-1024KiB, (W) 1024KiB-1024KiB, (T) 1024KiB-1024KiB, ioengine=libaio, iodepth=8
...
fio-3.7
Starting 8 processes
fiotest: Laying out IO file (1 file / 10240MiB)
^Cbs: 8 (f=8): [m(8)][86.7%][r=266MiB/s,w=256MiB/s][r=266,w=256 IOPS][eta 00m:16s]
fio: terminating on signal 2
Jobs: 8 (f=8): [m(8)][87.5%][r=238MiB/s,w=250MiB/s][r=238,w=250 IOPS][eta 00m:15s]
fiotest: (groupid=0, jobs=8): err= 0: pid=119: Wed May  4 01:59:49 2022
   read: IOPS=246, BW=246MiB/s (258MB/s)(10.6GiB/44238msec)
    slat (usec): min=10, max=110765, avg=3027.79, stdev=10297.34
    clat (msec): min=8, max=343, avg=92.82, stdev=35.80
     lat (msec): min=8, max=343, avg=95.85, stdev=37.22
    clat percentiles (msec):
     |  1.00th=[   26],  5.00th=[   44], 10.00th=[   51], 20.00th=[   66],
     | 30.00th=[   73], 40.00th=[   80], 50.00th=[   91], 60.00th=[  100],
     | 70.00th=[  106], 80.00th=[  121], 90.00th=[  138], 95.00th=[  157],
     | 99.00th=[  201], 99.50th=[  224], 99.90th=[  284], 99.95th=[  305],
     | 99.99th=[  326]
   bw (  KiB/s): min=10240, max=57344, per=12.51%, avg=31559.36, stdev=7650.58, samples=704
   iops        : min=   10, max=   56, avg=30.76, stdev= 7.46, samples=704
  write: IOPS=252, BW=252MiB/s (264MB/s)(10.9GiB/44238msec)
    slat (usec): min=60, max=196760, avg=14803.03, stdev=18096.68
    clat (msec): min=27, max=444, avg=145.26, stdev=53.50
     lat (msec): min=31, max=445, avg=160.07, stdev=55.94
    clat percentiles (msec):
     |  1.00th=[   61],  5.00th=[   75], 10.00th=[   86], 20.00th=[  101],
     | 30.00th=[  110], 40.00th=[  124], 50.00th=[  136], 60.00th=[  150],
     | 70.00th=[  167], 80.00th=[  186], 90.00th=[  220], 95.00th=[  249],
     | 99.00th=[  305], 99.50th=[  321], 99.90th=[  372], 99.95th=[  388],
     | 99.99th=[  414]
   bw (  KiB/s): min=18432, max=49152, per=12.48%, avg=32225.72, stdev=5107.40, samples=704
   iops        : min=   18, max=   48, avg=31.41, stdev= 5.00, samples=704
  lat (msec)   : 10=0.01%, 20=0.16%, 50=4.78%, 100=35.66%, 250=56.93%
  lat (msec)   : 500=2.46%
  cpu          : usr=0.42%, sys=0.69%, ctx=15497, majf=0, minf=81
  IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=99.7%, 16=0.0%, 32=0.0%, >=64=0.0%
     submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     complete  : 0=0.0%, 4=100.0%, 8=0.1%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
     issued rwts: total=10900,11151,0,0 short=0,0,0,0 dropped=0,0,0,0
     latency   : target=0, window=0, percentile=100.00%, depth=8

Run status group 0 (all jobs):
   READ: bw=246MiB/s (258MB/s), 246MiB/s-246MiB/s (258MB/s-258MB/s), io=10.6GiB (11.4GB), run=44238-44238msec
  WRITE: bw=252MiB/s (264MB/s), 252MiB/s-252MiB/s (264MB/s-264MB/s), io=10.9GiB (11.7GB), run=44238-44238msec
```

## Some Storage Facts

![](../../assets/images/longhorn/bench2.png) 

- Bandwidth and latency increase when the block size is higher. IOPS declines.

## Longhorn replica size 2 vs size 3 

The following graph illustrates the performance benchmarking between Longhorn volume with replica size 2 and replica size 3.

![](../../assets/images/longhorn/bench1.png) 

The result seemingly implies that there is no significant difference between replica size 2 and size 3 in a small cluster.