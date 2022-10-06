---
layout: default
title: Threading in Python
parent: Machine Learning
nav_order: 1
---

# Python Performance in Machine Learning
{: .no_toc }

Python has become the de-facto language framework for writing machine learning code due to its rich modules ecosystem. I like Python because it's human readable and offers wide range of easy-to-use libraries especially in the data science realm. This blog is articulated to explore ways to enhance the Python code performance in order to deliver quicker result for data scientists. 
By default, Python uses single CPU thread to execute the code. This is largely due to GIL (Global Interpreter Lock) as single thread is safe to prevent corrupted output as a result of race condition. While thread-safe is good, it does come with a price - the allocated/available CPU resource is underutilized and it takes longer time to run the code. Today, there are ways to improve the performance with the likes of using multithreading and multiprocessing modules but this must be done carefully because there's always a reason for GIL to exist. Let's run some experiments to find out more.

The following experiments are carried out using Cloudera Machine Learning (CML) on Openshift 4.8 with the hardware specification as shown below.

| CPU          | Intel(R) Xeon(R) Gold 5220R CPU @ 2.20GHz | 
| Memory  | DIMM DDR4 Synchronous Registered (Buffered) 2933 MHz (0.3 ns) | 
| Disk | SSD P4610 1.6TB SFF    | 

- TOC
{:toc}

---
## Single Thread in Python

Run and explore the outcome of the following experiment when a typical Python code executes with a single process thread.

1. Create a CML session with 1 CPU/2 GiB memory profile. Open a `Terminal Access` box and run the top command to check the total threads being used by the process ID of the session pod. In this example, the process ID is 94. Note that there are 30 threads being opened by the process of this running session pod.

    ```bash
    $ top -p 94 -H
    ```

    ![](../../assets/images/cml/singlethread1.png)    
 
2. Create a simple `input` file with the following content.

    ```yaml
    line1
    line2
    line3
    line4
    line5
    line6
    line7
    line8
    line9
    line10
    line11
    line12
    line13
    line14
    line15
    line16
    line17
    line18
    line19
    line20
    ```

3. Run this Python script. This script is coded to read line by line from the `input` file and write each line into the `output` file.
 
    ![](../../assets/images/cml/singlethread2.png)

4. Check the outcome of the `output` file.

    ```bash
    $ more output
    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line1

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line2

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line3

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line4

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line5

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line6

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line7

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line8

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line9

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line10

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line11

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line12

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line13

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line14

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line15

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line16

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line17

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line18

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line19

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line20 
    ```    

Observation #1: Python runs the code linearly with a single CPU process Mainthread. The output file also shows which CPU is scheduled to run the process thread at one point of time. As expected, the code writes each line number to the `output` file sequentially without data corruption.

# Multithreading with Threading Module in Python

Let's shift gear by executing the code using Threading Module in Python

5. Create a CML session with 1 CPU/2 GiB memory profile. Open a `Terminal Access` box and run the top command to check the total threads being used by the process ID of the session pod. In this example, the process ID is 94. Note that there are 30 threads being opened by the process of this running session pod.

    ```bash
    $ top -p 94 -H
    ```

    ![](../../assets/images/cml/singlethread1.png)    
 
6. Use the same `input` file as in step 2.

7. Run this Python script. Similar to the previous script, the script is augmented to make use of [Threading](https://docs.python.org/3/library/threading.html) class module.
 
    ![](../../assets/images/cml/singlethread2.png)

8. Check the outcome of the newly generated `output` file.

    ```bash
    $ more output
    current thread:<Thread(Thread-3, started 139792767305472)>
    current pid:94
    total threads:35
    CPU number:15
    write line1

    current thread:<Thread(Thread-5, started 139792750520064)>
    current pid:94
    total threads:35
    CPU number:15
    write line2

    current thread:<Thread(Thread-4, started 139792758912768)>
    current pid:94
    total threads:36
    CPU number:15
    write line3

    current thread:<Thread(Thread-7, started 139792528439040)>
    current pid:94
    total threads:36
    CPU number:15
    write line4

    current thread:<Thread(Thread-8, started 139792520046336)>
    current pid:94
    total threads:39
    CPU number:15
    write line5

    current thread:<Thread(Thread-6, started 139792742127360)>
    current pid:94
    total threads:38
    CPU number:15
    write line6

    current thread:<Thread(Thread-9, started 139792511653632)>
    current pid:94
    total threads:40
    CPU number:15
    write line7

    current thread:<Thread(Thread-10, started 139792503260928)>
    current pid:94
    total threads:40
    CPU number:15
    write line8

    current thread:<Thread(Thread-12, started 139792494868224)>
    current pid:94
    total threads:40
    CPU number:15
    write line8

    current thread:<Thread(Thread-17, started 139791924459264)>
    current pid:94
    total threads:42
    CPU number:15
    write line10

    current thread:<Thread(Thread-16, started 139792478082816)>
    current pid:94
    total threads:42
    CPU number:15
    write line11

    current thread:<Thread(Thread-13, started 139792750520064)>
    current pid:94
    total threads:43
    CPU number:15
    write line12

    current thread:<Thread(Thread-11, started 139792767305472)>
    current pid:94
    total threads:42
    CPU number:15
    write line13

    current thread:<Thread(Thread-15, started 139792758912768)>
    current pid:94
    total threads:42
    CPU number:15
    write line14

    current thread:<Thread(Thread-20, started 139792742127360)>
    current pid:94
    total threads:42
    CPU number:15
    write line16

    current thread:<Thread(Thread-14, started 139792486475520)>
    current pid:94
    total threads:41
    CPU number:15
    write line15

    current thread:<Thread(Thread-19, started 139792520046336)>
    current pid:94
    total threads:42
    CPU number:15
    write line18

    current thread:<Thread(Thread-22, started 139792528439040)>
    current pid:94
    total threads:39
    CPU number:15
    write line17

    current thread:<Thread(Thread-18, started 139791916066560)>
    current pid:94
    total threads:42
    CPU number:15
    write line19

    current thread:<Thread(Thread-21, started 139792511653632)>
    current pid:94
    total threads:36
    CPU number:15
    write line20
    ```
    
Observation #2: The above output shows the process uses multiple threads to run the code. However, unlike the previous outcome, the line numbers are no longer in sequence, e.g. line17 is written after line18 by Thread-22 and Thread-19 respectively. This is the behaviour of race condition as a result of multiple threads running the same process into the same file. So why bother to use multiple threads? Let's find out next.

# Single vs Multi Threading in Python

Let's shift gear by executing t

---

