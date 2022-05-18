---
layout: default
title: Cloudera Manager
parent: CDP Private Cloud
nav_order: 2
---

# Cloudera Manager (CM)
{: .no_toc }

This article explains the steps to install the CM.

- TOC
{:toc}

---

## Sanity Check

1. Ensure JDK has already been installed.

  ```bash
  # rpm -qa | grep jdk
    copy-jdk-configs-3.3-10.el7_5.noarch
    java-11-openjdk-11.0.14.1.1-1.el7_9.x86_64
    java-11-openjdk-headless-11.0.14.1.1-1.el7_9.x86_64
    java-11-openjdk-devel-11.0.14.1.1-1.el7_9.x86_64
  ```

## Compute, Storage and Network

1. TBA

## DNS Server

1. TBA

## NTP Server

1. TBA

## Kerberos Server

1. TBA

## LDAP Server

1. TBA

## Relational Database

1. The database requirements is described in this [link](https://docs.cloudera.com/cdp-private-cloud-base/7.1.7/installation/topics/cdpdc-database-requirements.html).


2. _Optional:_ Initialize search data (creates `search-data.json`)
  ```bash
  $ bundle exec just-the-docs rake search:init
  ```


3. Point your web browser to [link](https://docs.cloudera.com/cdp-private-cloud-base/7.1.7/installation/topics/cdpdc-database-requirements.html)