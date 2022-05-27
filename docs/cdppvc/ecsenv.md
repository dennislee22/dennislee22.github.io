---
layout: default
title: Setup ECS Environment
parent: CDP PvC How To ...
grand_parent: CDP Private Cloud
nav_order: 1
---

# Setup ECS Environment
{: .no_toc }

This article explains the step to setup the ECS environment so that user can administer the CDP PvC ECS cluster after successful installation.

---

1. Set up the ECS environment in the ECS master/server node.

    ```bash
    # cp /etc/rancher/rke2/rke2.yaml .kube/config
    # export PATH=$PATH:/var/lib/rancher/rke2/bin
    # kubectl get nodes
    NAME                     STATUS     ROLES                       AGE    VERSION
    ecsmaster1.cdpkvm.cldr   Ready      control-plane,etcd,master   120m   v1.21.8+rke2r2
    ecsworker1.cdpkvm.cldr   Ready      <none>                      117m   v1.21.8+rke2r2
    ecsworker2.cdpkvm.cldr   Ready      <none>                      117m   v1.21.8+rke2r2
    ```
    
2. Amend the ~/.bash_profile login shell to include `export PATH=$PATH:/var/lib/rancher/rke2/bin` parameter to persist the environment setting.   