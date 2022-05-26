---
layout: default
title: Setup ECS Environment
parent: CDP PvC How To ...
grand_parent: CDP Private Cloud
nav_order: 1
---

## Setup ECS Environment

1. Set up ECS environment in the ECS master/server node.

    ```bash
    # cp /etc/rancher/rke2/rke2.yaml .kube/config
    # export PATH=$PATH:/var/lib/rancher/rke2/bin
    # kubectl get nodes
    NAME                     STATUS     ROLES                       AGE    VERSION
    ecsmaster1.cdpkvm.cldr   Ready      control-plane,etcd,master   120m   v1.21.8+rke2r2
    ecsworker1.cdpkvm.cldr   Ready      <none>                      117m   v1.21.8+rke2r2
    ecsworker2.cdpkvm.cldr   Ready      <none>                      117m   v1.21.8+rke2r2
    ```