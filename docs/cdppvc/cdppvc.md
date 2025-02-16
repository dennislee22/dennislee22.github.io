---
layout: default
title: CDP Private Cloud
nav_order: 2
has_children: true
permalink: docs/cdppvc
---

# CDP Private Cloud
{: .no_toc }

![](../../assets/images/overall_arch.png)

- CDP Private Cloud solution delivers powerful analytic, transactional, and machine learning workloads in an on-premise environment. 
- It consists of 2 main components - CDP Private Cloud Base (CDP PvC Base) and CDP Private Cloud Data Services (CDP PvC DS). 
- Cloudera Manager (CM) acts as the single pane of management system that installs, manages, configures, and monitors the entire CDP Private Cloud solution.
- CDP PvC Base cluster stores data store options including HDFS and Ozone that serve as the data lake. It has high degree of consistent security and governance with SDX (Shared Data Experience) to enable safe and compliant data lakes with policy-based data access for users. 
- CDP PvC Base cluster also hosts other powerful big data services such as Hive, Kudu, Kafka, Solr, Hbase and many others. CDP PvC DS leverages Kubernetes technology for microservices cloud strategy by capitalizing on the benefits such as rapid deployment, portability and scalability. 
- As of time of writing, CDP PvC DS platform can host the following CDP PvC Data Services in which they are provisioned on the Kubernetes platform. 
    - Cloudera Data Warehouse (CDW)
    - Cloudera Machine Learning (CML)
    - Cloudera Data Engineering (CDE)
- Similar to CDP PvC Base services, these data services utilize Apache data analytics projects such as Impala, Hive and Spark. Apache Yunikorn is the latest addition as an universal resource scheduler for running Big Data/Machine Learning workloads on the Kubernetes platform.


