---
layout: default
title: Prerequisites
parent: CDP Private Cloud
nav_order: 1
---

# Prerequisites
{: .no_toc }
The following components need to be prepared prior to the installation of CDP Private Cloud Platform.

- TOC
{:toc}

---

## Cloudera Subscription

1. TBA

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
  ```yaml
  theme: "just-the-docs"
  ```

2. _Optional:_ Initialize search data (creates `search-data.json`)
  ```bash
  $ bundle exec just-the-docs rake search:init
  ```


3. Point your web browser to [link](https://docs.cloudera.com/cdp-private-cloud-base/7.1.7/installation/topics/cdpdc-database-requirements.html)