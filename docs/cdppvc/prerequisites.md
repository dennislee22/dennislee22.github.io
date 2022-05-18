---
layout: default
title: Prerequisites
parent: CDP Private Cloud
nav_order: 1
---

# Minimum Prerequisites
{: .no_toc }
The following components need to be prepared prior to the installation of CDP Private Cloud Platform.

- TOC
{:toc}

---

## Cloudera Subscription

1. Obtain a valid product subscription from Cloudera. Cloudera Manager requires a valid license to install accordingly. 

## Compute, Storage and Network

1. The hardware requirements are solely determined by the specific CDP services to be installed in both CDP Base and ECS.
2. The required minimum CDP Base services and its dependencies to install CML, CDW and CDE are illustrated in the following table.

![](../../assets/images/base_svc_table1.png)

3. Services such as HDFS, Zookeeper and [Ozone](https://docs.cloudera.com/cdp-private-cloud-upgrade/latest/release-guide/topics/cdpdc-ozone.html) have special storage requirements.

## DNS Server

1. An external DNS server must be able to route inbound traffic to both CDP Base platform and ECS. It must contain forward and reverse zones.

## NTP Server

1. All CDP Base and ECS nodes must be installed with NTP client and be able to synchronize the time with the external NTP server.

## Kerberos Server

1. An external Kerberos server and the Kerberos key distribution center (KDC) (with a realm established) must be available provide authentication to CDP services, users and hosts.
2. 

## LDAP Server

1. An external LDAP-compliant identity/directory server is required to enable the CDP solution to look up user accounts and groups in the directory.

## Relational Database

1. The database requirements is described in this [link](https://docs.cloudera.com/cdp-private-cloud-base/7.1.7/installation/topics/cdpdc-database-requirements.html).
2. This article uses PostgreSQL 12 database as the external database.