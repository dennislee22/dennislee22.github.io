---
layout: default
title: Multithreading in Python
parent: Machine Learning
nav_order: 1
---

# Multithreading in Python
{: .no_toc }

By default, Python uses single CPU thread to execute the code. This is largely due to GIL (Global Interpreter Lock) as single thread is safe to prevent corrupted output as a result of race condition. While thread-safe is good, it does come with a price - the allocated/available CPU resource is underutilized and it takes longer time to run the code. There are ways to improve the performance with the likes of using multithreading and multiprocessing modules but this must be done carefully because there's always a reason for GIL to exist. Let's run some experiments to find out more.

The following experiments are carried out using Cloudera Machine Learning (CML) on Openshift with the following hardware specification.

| CPU          | Intel(R) Xeon(R) Gold 5220R CPU @ 2.20GHz | 
| Memory  | DIMM DDR4 Synchronous Registered (Buffered) 2933 MHz (0.3 ns) | 
| Disk | SSD P4610 1.6TB SFF    | 

---
## Single Thread

Run the following experiment and explore the outcome when a typical Python code executes with a single process thread.

1. Create a CML session with 1 CPU/2 GiB memory profile. Open the `Terminal Access` and run the top command to check the total threads being used by the process ID of the session pod. In this example, the process ID is 94.

    ```bash
    $ top -p 94 -H
    ```

    ![](../../assets/images/cml/singlethread1.png)    
 
2. Create a simple input file with the following content.

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

2. Run this Python script.
 
    ![](../../assets/images/cml/singlethread2.png)

3. Run the following SQL queries twice and take note of the speed result.

    ```bash
    $ more output
    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line1

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line2

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line3

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line4

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line5

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line6

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line7

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line8

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line9

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line10

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line11

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line12

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line13

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line14

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line15

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line16

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line17

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line18

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line19

    current threads:<_MainThread(MainThread, started 140555361314624)>
    current pid:94
    total threads:30
    CPU number:6
    write line20 
    ```    
    
    
8. Repeat step 4 for file format Parquet by creating a table with the following schema.

    ```yaml
    CREATE TABLE db1.parquet(
    FirstName string, LastName string,    
    MSISDN bigint, DOB date, age int,
    Postcode int, City string)
    STORED AS parquet
    TBLPROPERTIES ('parquet.schema.literal'='{
    "name": "sample1",
    "type": "record",
    "fields": [
    {"name":"one", "type":"binary"},
    {"name":"two", "type":"binary"},
    {"name":"three", "type":"INT64"},
    {"name":"four", "type":"date"},
    {"name":"five", "type":"INT32"},
    {"name":"six", "type":"INT32"},
    {"name":"seven", "type":"binary"}
    ]}')
    ```

9. Run the following SQL queries twice and take note of the speed to run them completely.

    ```yaml
    SELECT COUNT (*) FROM db1.parquet;   
    ```    
    
    ```yaml
    SELECT AVG(age) FROM db1.parquet where lastname = 'Davis' and age > 30 and age < 40;
    ``` 

10. Repeat step 4 for file format Avro by creating a table with the following schema.

    ```yaml
    CREATE TABLE db1.avro(
    FirstName string, LastName string,    
    MSISDN bigint, DOB date, age int,
    Postcode int, City string)
    STORED AS avro
    TBLPROPERTIES ('avro.schema.literal'='{
    "name": "sample1",
    "type": "record",
    "fields": [
    {"name":"FirstName", "type":"string"},
    {"name":"LastName", "type":"string"},
    {"name":"MSISDN", "type":"long"},
    {"name":"DOB", "type":"string"},
    {"name":"age", "type":"int"},
    {"name":"Postcode", "type":"int"},
    {"name":"City", "type":"string"}
    ]}')
    ```    

11. Run the following SQL queries twice and take note of the speed to run them completely.

    ```yaml
    SELECT COUNT (*) FROM db1.avro;
    ```    

    ```yaml
    SELECT AVG(age) FROM db1.avro where lastname = 'Davis' and age > 30 and age < 40;
    ``` 

12. Access `Hue` tool of the `Impala` virtual warehouse. Create database `db2`.
   
 
13. Use the SQL Editor to create an external table in the database `db2`.
    

14. Create a managed table using the Parquet file format based on the schema as shown below.
    
    ```yaml
    CREATE TABLE db2.parquet2(
    FirstName string, LastName string,    
    MSISDN bigint, DOB date, age int,
    Postcode int, City string)
    STORED AS parquet
    TBLPROPERTIES ('parquet.schema.literal'='{
    "name": "sample1",
    "type": "record",
    "fields": [
    {"name":"one", "type":"binary"},
    {"name":"two", "type":"binary"},
    {"name":"three", "type":"INT64"},
    {"name":"four", "type":"date"},
    {"name":"five", "type":"INT32"},
    {"name":"six", "type":"INT32"},
    {"name":"seven", "type":"binary"}
    ]}')
    ```    

15. Insert the data from the external `tmp` table into this newly created Parquet-based table. Take note of the speed to execute this task completely.

    ```yaml
    INSERT INTO table db2.parquet2 SELECT * from db1.tmp;  
    ```    
    
16. Run the following SQL queries twice and take note of the speed result.

    ```yaml
    SELECT COUNT (*) FROM db2.parquet2;   
    ```    
    
    ```yaml
    SELECT AVG(age) FROM db2.parquet2 where lastname = 'Davis' and age > 30 and age < 40;
    ``` 
    
17. Repeat step 16 for `tmp` table with file format CSV.

    ```yaml
    SELECT COUNT (*) FROM db1.tmp;   
    ```    
    
    ```yaml
    SELECT AVG(age) FROM db1.tmp where lastname = 'Davis' and age > 30 and age < 40;
    ``` 
    
## Performance Result

- The following table shows the time taken (in seconds) to run each SQL query in the specific file format table without SNAPPY compression.
- By default, ZLIB compression is enabled for any managed ORC-based table in `Hive` engine in CDW unless specified.


| File Format  | Engine | INSERT | SELECT COUNT (1st)|SELECT COUNT (2nd) |SELECT AVG(1st)|SELECT AVG(2nd)|
|:-------------|:----------------|:------------------|:------------------|---------------|---------------|
| CSV          | Hive   | N.A.   |26.83              | 2.28              |44.2           |4.1            |
| ORC (ZLIB)   | Hive   | 507    |0.40               | 0.39              |8.13           |0.39           | 
| Avro         | Hive   | 355    |0.40               | 0.38              |207            |0.40           |
| Parquet      | Hive   | 332    |0.38               | 0.38              |11.78          |0.37           |
| Parquet      | Impala | 32     |0.36               | 0.35              |1.76           |1.62           |
| CSV          | Impala | N.A.   |3.12               | 1.75              |4.69           |4.1            |

## Conclusion

- In comparison to running the same SQL queries in other solution, CDW in CDP Private Cloud platform might take shorter duration to process the queries - thanks to its high-speed caching mechanism especially when running the same query repeatedly.
- Parquet stands out in terms of running the interactive SQL query at the fastest speed. As it is a pioneer file format for Impala, running SQL query in Parquet-based table in Impala produces quicker result compared to running the same query in Hive engine. Impala is endorsing Parquet while it has some [limitations](https://impala.apache.org/docs/build/html/topics/impala_file_formats.html) supporting other file formats.
- Although Parquet emerges as the winner in terms of performance, Avro and ORC are still very relevant for other use cases and objectives. Avro is popular for its schema evolution mechanism. ORC provides high efficiency in terms of storing the Hive data and is usually positioned for ETL/ELT process.

---