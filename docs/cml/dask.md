---
layout: default
title: Dask
parent: Machine Learning
nav_order: 10
---

# Dask
{: .no_toc }

Dask enhances Python performance using parallel computing concept. To do so, Dask offers a list of libraries that mimics the popular data science tools such as numpy and pandas. For instance, Dask arrays organize Numpy arrays, break them into chunks to complete the computation process in a parallel fashion. As a result, large dataset can be processed using multiple nodes as opposed to the typical single node resource. This article describes the behavioural outcome in terms of speed and CPU utilization when using Dask arrays with different chunk sizes.

The following experiments are carried out using Jupyterlab notebook in Cloudera Machine Learning (CML) on the Kubernetes platform powered by Openshift 4.8 with the hardware specification as described below. Here's the [link](https://github.com/dennislee22/machineLearning/blob/master/dask_cml.ipynb) to download the complete notebook.

| CPU          | Intel(R) Xeon(R) Gold 5220R CPU @ 2.20GHz | 
| Memory  | DIMM DDR4 Synchronous Registered (Buffered) 2933 MHz (0.3 ns) | 
| Disk | SSD P4610 1.6TB SFF    | 

- TOC
{:toc}

---
## Dask with Single Worker


- Create a Jupyterlab session with 2 CPU/16 GiB memory profile.

![](../../assets/images/cml/dask1.png)

{% jupyter_notebook "dask_cml.ipynb" %}

## Dask with Multiple Workers

Now let's use multiple workers to run the same code with the same intended outcome.

1. In the CML session, open a `Terminal Access` box and run the `top` command (type g4 to view the PPID).

2. Run the same Python code with `max_workers=3` and observe the processing time taken by Python to run the code. 

3. When the code is being executed, note that 3 child processes are spawned to execute the code concurrently.

    ![](../../assets/images/cml/mprocess3.png)    
 
4. It takes approximately 13 seconds with 3 workers to run the program.

    ```yaml
    Job Starts: 2408966.582076456
    Job Ends: 2408980.419085899
    Totals Execution Time:13.84 seconds.
    ```
    
5. Next, check the integrity of the output file. To reiterate, the purpose of this code is read line by line from the `input` file and write each line into the `output` file in a sequential manner. Run the following script. In the event of no output is produced, it shows the code achieves the intended outcome without data corruption. The result shows the same outcome as the previous test (using single worker).

    ```bash
    $ cnt=0;for i in `cat output | grep line`; do cnt=`expr $cnt + 1` ; if [ $i != line$cnt ]; then echo $i;fi ; done
    line1
    line1
    line4
    line2
    line5
    line2
    line3
    line6
    line3
    line4
    ```

    Excerpt from the `output` file:

    ```yaml
    current pid:154
    total threads:1
    CPU number:10
    line1

    current pid:154
    total threads:1
    CPU number:10
    line2

    current pid:154
    total threads:1
    CPU number:10
    line3

    current pid:158
    total threads:1
    CPU number:5
    line1

    current pid:157
    total threads:1
    CPU number:22
    line1
    ```

## Dask using GPU

Now let's use multiple workers to run the same code with the same intended outcome.

1. In the CML session, open a `Terminal Access` box and run the `top` command (type g4 to view the PPID).

2. Run the same Python code with `max_workers=3` and observe the processing time taken by Python to run the code. 

3. When the code is being executed, note that 3 child processes are spawned to execute the code concurrently.


Conclusion: Although concurrent.futures.ProcessPoolExecutor is able to complete the program at a faster speed with more workers in place, the outcome of the output file fails to deliver the intention of the code. The line number is not written sequentially. This is the behaviour of race condition as a result of multiple processes writing into the same file in parallel. In other words, concurrent.futures.ProcessPoolExecutor is not thread-safe as tt creates a pool of processes to execute calls asynchronously. In a nutshell, while multiprocessing is able to use the available/allocated CPU cores to run multiple processes in order to reduce the completion time, it is not suitable for every use case especially CPU-bound and long running task.

---


