---
layout: default
title: Embedded Container Service
parent: Installation
grand_parent: CDP Private Cloud
nav_order: 4
---

# Embedded Container Service (ECS) Installation
{: .no_toc }

This article explains the necessary steps to install Cloudera Manager (CM) on Centos7.9 OS which is [one of the supported operating systems](https://supportmatrix.cloudera.com/). Please ensure that the [prerequisites]({{ site.baseurl }}{% link docs/cdppvc/prerequisites.md %}) have already been prepared prior to running this procedure.

- TOC
{:toc}

---

## Run sanity check in each host.

1. JDK has already been installed.

    ```bash
    # rpm -qa | grep jdk
    copy-jdk-configs-3.3-10.el7_5.noarch
    java-11-openjdk-11.0.14.1.1-1.el7_9.x86_64
    java-11-openjdk-headless-11.0.14.1.1-1.el7_9.x86_64
    java-11-openjdk-devel-11.0.14.1.1-1.el7_9.x86_64
    ```

2. The external DNS server is able to resolve the hostname and perform reverse DNS lookup. Please this step for all the CDP PvC Base and ECS nodes.

1. Install LVM2 package.

    ```bash
    # yum install lvm2 -y
    ```
    
2. 