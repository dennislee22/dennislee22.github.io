---
layout: default
title: Demo Architecture
parent: CDP Private Cloud
has_children: false
nav_order: 1
---


## Demo Architecture
- The next subtopics will describe the end-to-end installation procedure which is articulated based on the following demo architecture diagram.

    ![](../../assets/images/logicalarch.png)


- In this demo, The Kubernetes platform to host CDP PvC Data Services is powered by the ECS (Embedded Container Service) platform.
- The CDP PvC Base hosts are provisioned with the minimum CDP PvC Base services that serve as the prerequisites before installing the CDP PvC Data Services on the ECS platform. 
- The placement of the services (role assignment) is based on the recommendation highlighted in this [link](https://docs.cloudera.com/cdp-private-cloud-base/7.1.7/installation/topics/cdpdc-runtime-cluster-hosts-role-assignments.html).
- Most of the CDP PvC Base services are installed in high availabiity mode and hence the aforementioned services are to be deployed in more than one host.
- Only 1 ECS master/server node is being in this demo but the latest CDP software version supports 3 ECS master/server nodes.

---    
   Next Step
   {: .label .label-blue } 
   
- Explore the end-to-end CDP Private Cloud installation procedure in the next [subtopic]({{ site.baseurl }}{% link docs/cdppvc/installation.md %}).
        
