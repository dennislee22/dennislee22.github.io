---
layout: default
title: Demo Architecture
parent: CDP Private Cloud
has_children: false
nav_order: 1
---


## Demo Architecture
- The next subtopics will describe the end-to-end CDP Private Cloud installation procedure using the following demo/proof-of-concept architecture diagram. 
- In this demo, the CDP PvC Data Services are hosted on the Kubernetes platform powered by the Openshift and/or the ECS (Embedded Container Service) platform. Both are mutually exclusive whereby CDP Private Cloud solution requires either one of them. Also, CDP PvC solution has the capability to integrate the CDP Base cluster with both of them. 
- CM installs the minimum CDP PvC Base services in the CDP PvC Base hosts. These services serve as the prerequisites prior to installing the specific CDP PvC Data Service(s) on the Kubernetes platform. Further details will be explained in the subsequent subtopic.

   CDP Private Cloud Base
   {: .label .label-blue } 
    ![](../../assets/images/basearch.png)

- This demo environment has 8 servers hosting the minimum CDP PvC Base services.
- Most of the CDP PvC Base services are installed in high availabity mode and hence the aforementioned services are to be deployed in more than one host.
- The placement of the CDP PvC Base services (role assignment) is based on the recommendation highlighted in this [link](https://docs.cloudera.com/cdp-private-cloud-base/7.1.7/installation/topics/cdpdc-runtime-cluster-hosts-role-assignments.html).

   CDP Private Cloud Base with Openshift
   {: .label .label-blue } 
    ![](../../assets/images/ocparch.png)

- The Openshift environment in this demo environment comprises of 3 servers collating both Openshift master and worker roles whereby such design is not suitable for production environment. In the actual production environment, the required number of worker nodes depends heavily on the actual dimensioning input and master nodes are not supposed to host the production workloads and the [Openshift Container Storage](https://access.redhat.com/documentation/en-us/red_hat_openshift_container_storage/4.7). 

   CDP Private Cloud Base with ECS
   {: .label .label-blue } 
    ![](../../assets/images/ecsarch.png)    


- The ECS environment in this demo environment comprises of 1 master node and 2 ECS worker/agent nodes. In the actual production environment, the required number of ECS worker nodes depends heavily on the actual dimensioning input and how the failover mechanism is designed accordingly.

---    
   Next Step
   {: .label .label-blue } 
   
- Explore the installation prerequisites for the CDP Private Cloud in the next [subtopic]({{ site.baseurl }}{% link docs/cdppvc/prerequisites.md %}).
        
