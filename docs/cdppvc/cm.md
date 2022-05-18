---
layout: default
title: Cloudera Manager
parent: CDP Private Cloud
nav_order: 2
---

# Cloudera Manager (CM)
{: .no_toc }

This article explains the steps to install the CM. Please ensure that the [prerequisites](({{ site.baseurl }}{% link docs/prerequisites.md %})) have already been prepared prior to running this procedure.

- TOC
{:toc}

---

## Run sanity check inside CM host.

1. Ensure JDK has already been installed.

  ```bash
# rpm -qa | grep jdk
copy-jdk-configs-3.3-10.el7_5.noarch
java-11-openjdk-11.0.14.1.1-1.el7_9.x86_64
java-11-openjdk-headless-11.0.14.1.1-1.el7_9.x86_64
java-11-openjdk-devel-11.0.14.1.1-1.el7_9.x86_64
  ```

2. Ensure that the external DNS server is able to resolve the hostname and perform reverse DNS lookup. Please this step for all the CDP PvC Base and ECS nodes.

  ```bash
# nslookup idm
Server:		10.15.4.150
Address:	10.15.4.150#53

Name:	idm.cdpkvm.cldr
Address: 10.15.4.150

# nslookup 10.15.4.150
150.4.15.10.in-addr.arpa	name = idm.cdpkvm.cldr.
  ```

3. Ensure that the NTP client is schronizing the time with the external NTP server.