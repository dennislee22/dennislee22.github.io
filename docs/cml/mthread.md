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

    ```bash
    $ > input;for i in {1..20};do echo line$i >> input;done; head -20 input
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

4. Check the outcome of the `output` file. The output file also shows which CPU is scheduled to run the process thread at one point of time. As expected, the code writes each line number to the `output` file sequentially without data corruption as Python runs the code linearly with a single Mainthread process.

    ```bash
    $ more output
    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line1

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line2

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line3

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line4

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line5

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line6

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line7

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line8

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line9

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line10

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line11

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line12

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line13

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line14

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line15

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line16

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line17

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line18

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line19

    current thread:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    line20 
    ```    

5. Let's recreate more entries in the file and rerun the Python code.

    ```bash
    $ > input;for i in {1..10000};do echo line$i >> input;done; head -20 input
    ```
    
6. Observe the processing time taken by Python to run the code. Run a few times to ensure the result is consistent.

    ```yaml
    Job Starts: 2071176.127430943
    Job Ends: 2071208.742637604
    Totals Execution Time:32.62 seconds.
    ```

# Multithreading with Threading Module in Python

Let's shift gear by executing the code using Threading Module in Python.

1. Create a CML session with 1 CPU/2 GiB memory profile. Open a `Terminal Access` box and run the top command to check the total threads being used by the process ID of the session pod. In this example, the process ID is 94. Note that there are 30 threads being opened by the process of this running session pod.

    ```bash
    $ top -p 94 -H
    ```  
 
2. Use the same `input` file with 10000 entries.

3. Run this Python script. Similar to the previous script, the script is amended to make use of [Threading](https://docs.python.org/3/library/threading.html) class module.

4. Check the outcome of the newly generated `output` file. It shows the process uses multiple threads to run the code.

    ```bash
    $ more output
    current thread:<Thread(Thread-3, started 139772086433536)>
    current pid:97
    total threads:31
    CPU number:1
    line1

    current thread:<Thread(Thread-4, started 139772086433536)>
    current pid:97
    total threads:31
    CPU number:1
    line2

    current thread:<Thread(Thread-5, started 139772086433536)>
    current pid:97
    total threads:31
    CPU number:1
    line3

    current thread:<Thread(Thread-6, started 139772086433536)>
    current pid:97
    total threads:31
    CPU number:1
    line4

    current thread:<Thread(Thread-7, started 139772086433536)>
    current pid:97
    total threads:31
    CPU number:1
    line5

    current thread:<Thread(Thread-8, started 139772086433536)>
    current pid:97
    total threads:31
    CPU number:1
    line6

    current thread:<Thread(Thread-9, started 139772086433536)>
    current pid:97
    total threads:31
    CPU number:1
    line7

    current thread:<Thread(Thread-10, started 139772086433536)>
    current pid:97
    total threads:31
    CPU number:1
    line8

    current thread:<Thread(Thread-11, started 139772086433536)>
    current pid:97
    total threads:31
    CPU number:1
    line9
    ```
    
5. Run the following script to check if the code writes each line number to the `output` file sequentially without data corruption. The result shows same outcome as the previous test (using single thread).

    ```bash
    $ cnt=0;for i in `cat output | grep line`; do cnt=`expr $cnt + 1` ; if [ $i != line$cnt ]; then echo $i;fi ; done
    ```

6. The processing time taken by Python to run this code with multithreading is longer! Shouldn't it be quicker than running with a single thread?

    ```yaml
    Job Starts: 2071685.346476694
    Job Ends: 2071735.755200198
    Totals Execution Time:50.41 seconds.
    ```

# concurrent.futures.ThreadPoolExecutor in Python


Now let's use concurrent.futures.ThreadPoolExecutor module to run the code with the same objective which is writing each line number to the `output` file sequentially.

1. The processing time taken by Python to run the code with concurrent.futures.ThreadPoolExecutor module is shorter! 

    ```yaml
    Job Starts: 2070084.875582402
    Job Ends: 2070109.707474271
    Totals Execution Time:24.83 seconds.
    ```

2. Check the outcome of the newly generated `output` file. It shows the process spawns 10 threads (defined by number of workers) to run the code. The total threads before the running code was 30 and has increased to 40 when executing the code.

    ![](../../assets/images/cml/threadpool1.png)
    
    ```bash
    current thread:<Thread(ThreadPoolExecutor-0_1, started 139664655841024)>
    current pid:98
    total threads:36
    CPU number:2
    line1

    current thread:<Thread(ThreadPoolExecutor-0_3, started 139664639055616)>
    current pid:98
    total threads:36
    CPU number:2
    line2

    current thread:<Thread(ThreadPoolExecutor-0_0, started 139664664233728)>
    current pid:98
    total threads:36
    CPU number:2
    line3

    current thread:<Thread(ThreadPoolExecutor-0_2, started 139664647448320)>
    current pid:98
    total threads:38
    CPU number:2
    line4

    current thread:<Thread(ThreadPoolExecutor-0_5, started 139664283399936)>
    current pid:98
    total threads:40
    CPU number:2
    line5

    current thread:<Thread(ThreadPoolExecutor-0_1, started 139664655841024)>
    current pid:98
    total threads:40
    CPU number:2
    line6

    current thread:<Thread(ThreadPoolExecutor-0_4, started 139664630662912)>
    current pid:98
    total threads:40
    CPU number:2
    line7

    current thread:<Thread(ThreadPoolExecutor-0_6, started 139664275007232)>
    current pid:98
    total threads:40
    CPU number:2
    line8

    current thread:<Thread(ThreadPoolExecutor-0_8, started 139664258221824)>
    current pid:98
    total threads:40
    CPU number:2
    line9

    current thread:<Thread(ThreadPoolExecutor-0_0, started 139664664233728)>
    current pid:98
    total threads:40
    CPU number:2
    line10

    current thread:<Thread(ThreadPoolExecutor-0_4, started 139664630662912)>
    current pid:98
    total threads:40
    CPU number:2
    line11

    current thread:<Thread(ThreadPoolExecutor-0_9, started 139664249829120)>
    current pid:98
    total threads:40
    CPU number:2
    line12

    current thread:<Thread(ThreadPoolExecutor-0_3, started 139664639055616)>
    current pid:98
    total threads:40
    CPU number:2
    line13

    current thread:<Thread(ThreadPoolExecutor-0_7, started 139664266614528)>
    current pid:98
    total threads:40
    CPU number:2
    line14
    ```
    
5. Now, let's do the next check by checking integrity of the output. Run the following script to check if the code writes each line number to the `output` file sequentially. 

    ```bash
    $ cnt=0;for i in `cat output | grep line`; do cnt=`expr $cnt + 1` ; if [ $i != line$cnt ]; then echo $i;fi ; done
    line26
    line25
    line41
    line40
    line84
    line83
    line96
    line95
    ```
    
    Excerpt from the `output` file:

    
    ```yaml
    current thread:<Thread(ThreadPoolExecutor-1_9, started 139664249829120)>
    current pid:98
    total threads:40
    CPU number:2
    line26

    current thread:<Thread(ThreadPoolExecutor-1_8, started 139664283399936)>
    current pid:98
    total threads:40
    CPU number:2
    line25

    current thread:<Thread(ThreadPoolExecutor-1_0, started 139664258221824)>
    current pid:98
    total threads:40
    CPU number:2
    line27
    ```

6. The outcome is however not in line with the objective of the code - the line number is not written sequentially beginning from line26. Unlike the previous outcome, the line numbers are no longer in sequence, e.g. line25 is written after line26 by ThreadPoolExecutor-1_8 and ThreadPoolExecutor-1_9 respectively. This is the behaviour of race condition as a result of multiple threads running the same process into the same file.
    




---

