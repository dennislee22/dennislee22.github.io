---
layout: default
title: CDP Base
parent: Installation
grand_parent: CDP Private Cloud
nav_order: 3
---

# CDP Base Installation
{: .no_toc }

This article explains the necessary steps to install the minimum services on CDP Base platform. Please ensure that the [prerequisites]({{ site.baseurl }}{% link docs/cdppvc/prerequisites.md %}) have already been prepared and [CM]({{ site.baseurl }}{% link docs/cdppvc/cm.md %}) has already been installed successfully prior to running this procedure.

- TOC
{:toc}

---

## Run sanity check in each host

1. JDK has already been installed.

    ```bash
    # rpm -qa | grep jdk
    copy-jdk-configs-3.3-10.el7_5.noarch
    java-11-openjdk-11.0.14.1.1-1.el7_9.x86_64
    java-11-openjdk-headless-11.0.14.1.1-1.el7_9.x86_64
    java-11-openjdk-devel-11.0.14.1.1-1.el7_9.x86_64
    ```

2. The external DNS server is able to resolve the hostname and perform reverse DNS lookup. Please this step for all the CDP PvC Base and ECS nodes.

    ```bash
    # nslookup idm
    Server:		10.15.4.150
    Address:	10.15.4.150#53

    Name:	idm.cdpkvm.cldr
    Address: 10.15.4.150

    # nslookup 10.15.4.150
    150.4.15.10.in-addr.arpa	name = idm.cdpkvm.cldr.
    ```

3. NTP client is synchronizing time with the external NTP server.

4. Each host has already been registered with the external Kerberos server.

    ```bash
    # ipa host-show bmaster1
    Host name: bmaster1.cdpkvm.cldr
    Principal name: host/bmaster1.cdpkvm.cldr@CDPKVM.CLDR
    Principal alias: host/bmaster1.cdpkvm.cldr@CDPKVM.CLDR
    SSH public key fingerprint: SHA256:dyShLpzkqlRHc2LHiqXDbhM8ynT7v4yjZP4CZ212tqU root@bmaster1.cdpkvm.cldr (ssh-rsa),
                              SHA256:C+BAHEBbVAXfhUIpdFxoL2MOkF5pUGATuKnFQXCgJnc root@bmaster1.cdpkvm.cldr (ssh-rsa),
                              SHA256:/COofNFRyGmwAGR6sfonAcXtc/Knjs5/an1+SMX/8GA (ecdsa-sha2-nistp256), SHA256:OL8ZeU7+2E4yl7rsvKftXYTM7Bvr8fEVuxQaQBouwwo
                              (ssh-ed25519)
    Password: False
    Keytab: True
    Managed by: bmaster1.cdpkvm.cldr
    ```

## Install services on CDP Base via CM

1. Navigate to `Clusters` > `Add Cluster`.

    ![](../../assets/images/cdpbase/addbase1.png)

2. Select `Private Cloud Base Cluster` and click `Continue` button.

    ![](../../assets/images/cdpbase/addbase2.png)

3. Enter the Cluster Name and click `Continue` button. 

    ![](../../assets/images/cdpbase/addbase3.png)



