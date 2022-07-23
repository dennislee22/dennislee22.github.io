---
layout: default
title: SNAPPY Compression
parent: Data Warehousing
nav_order: 2

---
# SNAPPY Compression for Parquet, ORC, Avro
{: .no_toc }

- This article describes the steps to gauge the query performance and the storage output upon applying SNAPPY compression on the database tables with Avro, ORC and Parquet file formats in both Hive LLAP and Impala query engine using Cloudera Data Warehouse (CDW) in CDP Private Cloud platform.

- TOC
{:toc}

---


## Prerequisites

- The performance benchmarking tests are carried out using CDW on the CDP PvC (Openshift) platform with the following hardware specification.

| CPU          | Intel(R) Xeon(R) Gold 5220R CPU @ 2.20GHz | 
| Memory  | DIMM DDR4 Synchronous Registered (Buffered) 2933 MHz (0.3 ns) | 
| Disk | SSD P4610 1.6TB SFF    | 


- A sample data of 300 million CSV rows is produced using a python script with the [faker](https://faker.readthedocs.io/en/master/) generator. The schema of each row is sequenced as `Lastname, Firstname, MSISDN, Date of Birth, Postcode, City` as illustrated below.

    ```yaml
    Maria,Harmon,32378521,1998-11-14,17,30766,Durhammouth
    Anne,Adams,29481072,1982-10-28,36,70830,Deannabury
    Deborah,Sanders,21125797,2002-04-07,56,63993,New Ronaldland
    ```

- Copy the file to the HDFS cluster.

    ```bash
    # hdfs dfs -put 300mil.csv /tmp/sampledata/
    
    # hdfs dfs -du -h /tmp/sampledata/
    16.0 G  47.9 G  /tmp/sampledata/300mil.csv    
    ```

- In CDW, create a `Hive LLAP` and an `Impala` virtual warehouse with only 1 executor each.

    ![](../../assets/images/cdw/cdwfs1.png)

## Testing Procedure using Hive LLAP

1. Access `Hue` tool of the `Hive LLAP` virtual warehouse. Create database `db1`.

    ![](../../assets/images/cdw/cdwfs2.png)    
 
2. Use the SQL Editor to create an external table in the database `db1`.
 
    ![](../../assets/images/cdw/cdwfs3.png)       

3. Execute the following command and take note of the speed result. Repeat running the same command and jot down the result again.
    
    ![](../../assets/images/cdw/cdwfs4.png)
    
4. Create a Hive managed table using the ORC file format with SNAPPY compression as illustrated below.
    

    ```yaml
    CREATE TABLE db1.orc_snappy(
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
    {"name":"three", "type":"bigint"},
    {"name":"four", "type":"binary"},
    {"name":"five", "type":"int"},
    {"name":"six", "type":"int"},
    {"name":"seven", "type":"binary"}
    ]}');
    ```
  
5. Insert the data from the external `tmp` table into this newly created ORC-based table. Take note of the speed to execute this task completely.

    ```yaml
    INSERT INTO TABLE db1.orc_snappy SELECT * FROM tmp;
    ```
    
6. Check the result of the loaded data.    

    ![](../../assets/images/cdw/cdwfs7.png)
    

7. Run the following SQL queries twice and take note of the speed result.

    ```yaml
    SELECT COUNT (*) FROM db1.orc;   
    ```    
    
    ```yaml
    SELECT AVG(age) FROM db1.orc where lastname = 'Davis' and age > 30 and age < 40;
    ``` 
    
8. Repeat step 4 to 6 for file format Parquet using the table with following schema.

    ```yaml
    CREATE TABLE db1.parquet(
    FirstName string, LastName string,    
    MSISDN bigint, DOB date, age int,
    Postcode int, City string)
    STORED AS parquet
    TBLPROPERTIES ("parquet.compression"="SNAPPY",'parquet.schema.literal'='{
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

9. Run the following SQL queries twice and take note of the speed result.

    ```yaml
    SELECT COUNT (*) FROM db1.parquet;   
    ```    
    
    ```yaml
    SELECT AVG(age) FROM db1.parquet where lastname = 'Davis' and age > 30 and age < 40;
    ``` 

10. Repeat step 4 to 6 for file format Avro using the table with following schema.

    ```yaml
set hive.exec.compress.output=true;
set avro.output.codec=snappy;
CREATE TABLE db1.avro_snappy(
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
  ]}');
    ```
    
11. Run the following SQL queries twice and take note of the speed result.

    ```yaml
    SELECT COUNT (*) FROM db1.avro;
    ```    

    ```yaml
    SELECT AVG(age) FROM avro where lastname = 'Davis' and age > 30 and age < 40;
    ``` 


    
## Performance Result

- The following table shows the time taken (in seconds) to run each SQL query and its associated file format without SNAPPY compression. This result is adapted from the previous test as described [here]({{ site.baseurl }}{% link docs/cdw/benchmarkfs.md %}).


| File Format  | Engine | INSERT | SELECT COUNT (1st)|SELECT COUNT (2nd) |SELECT AVG(1st)|SELECT AVG(2nd)|
|:-------------|:----------------|:------------------|:------------------|---------------|---------------|
| ORC          | Hive   | 507    |0.40               | 0.39              |8.13           |0.39           | 
| Avro         | Hive   | 513    |0.40               | 0.38              |287            |0.40           |
| Parquet      | Hive   | 332    |0.38               | 0.38              |11.78          |0.37           |
| Parquet      | Parquet| 32     |0.36               | 0.35              |1.76           |0.37           |


- The following table shows the time taken (in seconds) to run each SQL query and its associated file format with SNAPPY compression.

| File Format  | Engine | INSERT | SELECT COUNT (1st)|SELECT COUNT (2nd) |SELECT AVG(1st)|SELECT AVG(2nd)|
|:-------------|:----------------|:------------------|:------------------|---------------|---------------|
| ORC          | Hive   | 1    |0.40              | 0.39            |1           |1          | 
| Avro         | Hive   | 352    |0.40               | 0.38              |329            |0.39           |
| Parquet      | Hive   | 352    |0.38               | 0.38              |9.64           |0.37           |
| Parquet      | Parquet| 1     |1              | 1             |1          |1           |


## Storage Output

- The following output shows the HDFS storage size of the generated tables (with and without SNAPPY compression) for each file format. 

    ```bash
    # hdfs dfs -du -h /warehouse/tablespace/managed/hive/db1.db
    13.0 G  38.9 G  /warehouse/tablespace/managed/hive/db1.db/avro
    4.1 G   12.4 G  /warehouse/tablespace/managed/hive/db1.db/orc
    9.7 G   29.0 G  /warehouse/tablespace/managed/hive/db1.db/parquet 
    ```   
    
    ```bash
    # hdfs dfs -du -h /warehouse/tablespace/managed/hive/db2.db
    6.4 G  19.3 G  /warehouse/tablespace/managed/hive/db2.db/parquet2
    ```    


# hive --orcfiledump /warehouse/tablespace/managed/hive/db1.db/orc/delta_0000001_0000001_0000/bucket_00000_0
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p0.15945976/jars/log4j-slf4j-impl-2.13.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p0.15945976/jars/slf4j-log4j12-1.7.30.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
WARNING: Use "yarn jar" to launch YARN applications.
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p0.15945976/jars/log4j-slf4j-impl-2.13.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p0.15945976/jars/slf4j-log4j12-1.7.30.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Processing data file /warehouse/tablespace/managed/hive/db1.db/orc/delta_0000001_0000001_0000/bucket_00000_0 [length: 243634496]
Structure for /warehouse/tablespace/managed/hive/db1.db/orc/delta_0000001_0000001_0000/bucket_00000_0
File Version: 0.12 with ORC_135
Rows: 16438711
Compression: ZLIB
Compression size: 32768
Calendar: Julian/Gregorian
Type: struct<operation:int,originalTransaction:bigint,bucket:int,rowId:bigint,currentTransaction:bigint,row:struct<firstname:string,lastname:string,msisdn:bigint,dob:date,age:int,postcode:int,city:string>>