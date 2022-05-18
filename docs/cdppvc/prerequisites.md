---
layout: default
title: Minimum Prerequisites
parent: CDP Private Cloud
nav_order: 1
---

# Minimum Prerequisites
{: .no_toc }
The following components are the minimum prerequisites to install the CDP Private Cloud Platform.

- TOC
{:toc}

---

## Cloudera Subscription

1. Obtain a valid product subscription from Cloudera. Cloudera Manager requires a valid license to install accordingly. 

## BareMetal Host

1. The hardware requirements are solely determined by the specific CDP services to be installed in both CDP Base and ECS.
2. The required minimum CDP Base services and its dependencies to install CML, CDW and CDE are illustrated in the following table.

![](../../assets/images/base_svc_table1.png)

3. Services such as HDFS, Zookeeper and [Ozone](https://docs.cloudera.com/cdp-private-cloud-upgrade/latest/release-guide/topics/cdpdc-ozone.html) have special storage requirements.
4. The supported OS is listed [here](https://docs.cloudera.com/cdp-private-cloud-base/7.1.7/installation/topics/cdpdc-os-requirements.html).
5. [JDK](https://docs.cloudera.com/cdp-private-cloud-base/7.1.7/installation/topics/cdpdc-java-requirements.html) must be installed in each host.
6. [Data at Rest](https://docs.cloudera.com/cdp-private-cloud-base/7.1.7/installation/topics/cdpdc-data-at-rest-encryption-requirements.html) is not covered in this article.

## DNS Server

1. An external DNS server must be able to route inbound traffic to both CDP Base platform and ECS. It must contain forward and reverse zones.

## NTP Server

1. All CDP Base and ECS nodes must be installed with NTP client and be able to synchronize the time with the external NTP server.

## Kerberos Server

1. An external Kerberos server and the Kerberos key distribution center (KDC) (with a realm established) must be available provide authentication to CDP services, users and hosts.

## LDAP Server

1. An external LDAP-compliant identity/directory server is required to enable the CDP solution to look up user accounts and groups in the directory.

## Relational Database

1. The database requirements is described in this [link](https://docs.cloudera.com/cdp-private-cloud-base/7.1.7/installation/topics/cdpdc-database-requirements.html).
2. This article uses PostgreSQL 12 database as the external database.
3. Create the following databases with users and its associated privileges. Note that simple passwords are being used here but the actual production environment should make use of complex passwords.

  ```yaml
CREATE ROLE scm LOGIN PASSWORD 'scm';
CREATE ROLE rman LOGIN PASSWORD 'rman';
CREATE ROLE hue LOGIN PASSWORD 'hue';
CREATE ROLE metastore LOGIN PASSWORD 'metastore';
CREATE ROLE oozie LOGIN PASSWORD 'oozie';
CREATE ROLE schemaregistry LOGIN PASSWORD 'schemaregistry';
CREATE ROLE smm LOGIN PASSWORD 'smm';
CREATE DATABASE scm OWNER scm ENCODING 'UTF8';
CREATE DATABASE rman OWNER rman ENCODING 'UTF8';
CREATE DATABASE hue OWNER hue ENCODING 'UTF8';
CREATE DATABASE metastore OWNER metastore ENCODING 'UTF8';
CREATE DATABASE oozie OWNER oozie ENCODING 'UTF8';
CREATE DATABASE schemaregistry OWNER schemaregistry ENCODING 'UTF8';
CREATE DATABASE smm OWNER smm ENCODING 'UTF8';
ALTER DATABASE metastore SET standard_conforming_strings=off;
ALTER DATABASE oozie SET standard_conforming_strings=off;
CREATE DATABASE ranger ENCODING 'UTF8';
CREATE USER rangeradmin WITH PASSWORD 'rangeradmin';
GRANT ALL PRIVILEGES ON DATABASE ranger TO rangeradmin;
CREATE DATABASE streamsmsgmgr;
CREATE USER streamsmsgmgr WITH PASSWORD 'streamsmsgmgr';
GRANT ALL PRIVILEGES ON DATABASE "streamsmsgmgr" to streamsmsgmgr;
CREATE USER cdpadmin WITH PASSWORD 'cdpadmin';
CREATE DATABASE dbenv OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE dbmlx OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE dbdwx OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE dbliftie OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE dbdex OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE dbresourcepoolmanager OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE dbclusteraccessmanager OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE dbalerts OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE dbums OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE cmregistration OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE clusterproxy OWNER cdpadmin ENCODING 'UTF8';
CREATE DATABASE registry;
CREATE USER registry WITH PASSWORD 'registry';
GRANT ALL PRIVILEGES ON DATABASE "registry" to registry;
  ```