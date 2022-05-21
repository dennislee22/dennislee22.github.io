---
layout: default
title: Cloudera Machine Learning
parent: Data Services
grand_parent: CDP Private Cloud
nav_order: 1
---

# Cloudera Machine Learning
{: .no_toc }

Blah

- TOC
{:toc}

---





    ```bash
    # kubectl get pv | head -1 ; kubectl get pv | grep cdp-env-1
    NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                                                                                                   STORAGECLASS   REASON   AGE
    pvc-7022f231-463b-4622-8a21-15f7a729b7d1   2Gi        RWO            Delete           Bound    cdp-env-1-d668dab9-monitoring-platform/storage-volume-monitoring-prometheus-alertmanager-0                              longhorn                66s
    pvc-946e99e2-0ef1-4ba5-8504-246f02c3142f   60Gi       RWO            Delete           Bound    cdp-env-1-d668dab9-monitoring-platform/monitoring-prometheus-server                                                     longhorn                68s
    pvc-c96965bb-b564-4cb5-b373-0fbcc7ead110   2Gi        RWO            Delete           Bound    cdp-env-1-d668dab9-monitoring-platform/storage-volume-monitoring-prometheus-alertmanager-1                              longhorn                11s
    ```

    ```bash
    # kubectl get ns
    NAME                                     STATUS   AGE
    cdp                                      Active   8h
    cdp-env-1-d668dab9-monitoring-platform   Active   2m16s
    default                                  Active   8h
    default-43e25dc6-monitoring-platform     Active   7h55m
    ecs-webhooks                             Active   8h
    infra-prometheus                         Active   8h
    kube-node-lease                          Active   8h
    kube-public                              Active   8h
    kube-system                              Active   8h
    kubernetes-dashboard                     Active   8h
    local-path-storage                       Active   8h
    longhorn-system                          Active   8h
    shared-services                          Active   3h50m
    vault-system                             Active   8h
    yunikorn                                 Active   8h
    ```

    ```bash
    # kubectl -n cdp-env-1-d668dab9-monitoring-platform get pods
    NAME                                                        READY   STATUS    RESTARTS   AGE
    monitoring-cm-health-exporter-59dc48c4d-twhnj               3/3     Running   0          2m56s
    monitoring-logger-alert-receiver-c89955fb7-zgxn5            2/2     Running   0          2m56s
    monitoring-metrics-server-exporter-94c45955b-pwsb6          2/2     Running   0          2m56s
    monitoring-platform-proxy-c5f8f5578-zglkb                   2/2     Running   0          2m56s
    monitoring-prometheus-alertmanager-0                        3/3     Running   0          2m56s
    monitoring-prometheus-alertmanager-1                        3/3     Running   0          119s
    monitoring-prometheus-kube-state-metrics-66f7f6f9b4-tzrfs   2/2     Running   0          52s
    monitoring-prometheus-pushgateway-64b5765874-9n8c4          2/2     Running   0          2m56s
    monitoring-prometheus-server-c4b8b4456-8lcz9                3/3     Running   0          2m56s
    snmp-notifier-86c7564b69-bkdwm                              2/2     Running   0          52s
    ```

    ```bash
    # kubectl -n cdp-env-1-d668dab9-monitoring-platform get pvc
    NAME                                                  STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
    monitoring-prometheus-server                          Bound    pvc-946e99e2-0ef1-4ba5-8504-246f02c3142f   60Gi       RWO            longhorn       3m13s
    storage-volume-monitoring-prometheus-alertmanager-0   Bound    pvc-7022f231-463b-4622-8a21-15f7a729b7d1   2Gi        RWO            longhorn       3m13s
    storage-volume-monitoring-prometheus-alertmanager-1   Bound    pvc-c96965bb-b564-4cb5-b373-0fbcc7ead110   2Gi        RWO            longhorn       2m16s
    ```

    ```bash
    # kubectl -n workspace1 get pvc
    NAME                                    STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
    livelog-data-livelog-0                  Bound    pvc-fd0b773a-e785-4b5c-afc1-fcd98afcc56b   1Ti        RWO            longhorn       16s
    model-metrics-data-model-metrics-db-0   Bound    pvc-05b25f2a-29cd-4341-8c69-a83c7e70efef   100Gi      RWO            longhorn       16s
    persist-dir-secret-generator-0          Bound    pvc-a16af508-e380-4243-944c-1473d2a6b711   10Mi       RWO            longhorn       16s
    postgres-data-versioned-db-0            Bound    pvc-39d799cf-6773-46f6-8f16-16778e008824   1Ti        RWO            longhorn       16s
    projects-pvc                            Bound    projects-share-workspace1                  1Ti        RWX                           16s
    s2i-git-server-repos-s2i-git-server-0   Bound    pvc-cb5c9ac7-0201-44be-aeb5-dec057db5870   1Ti        RWO            longhorn       16s
    s2i-queue-pvc                           Bound    pvc-2394a6d2-eecf-452e-b17f-4ab477bb1310   2Gi        RWO            longhorn       16s
    ```

    ```bash
    # kubectl get pv | head -1 ; kubectl get pv | grep workspace1
    NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                                                                                                   STORAGECLASS   REASON   AGE
    projects-share-workspace1                  1Ti        RWX            Retain           Bound    workspace1/projects-pvc                                                                                                                         49s
    pvc-05b25f2a-29cd-4341-8c69-a83c7e70efef   100Gi      RWO            Delete           Bound    workspace1/model-metrics-data-model-metrics-db-0                                                                        longhorn                36s
    pvc-2394a6d2-eecf-452e-b17f-4ab477bb1310   2Gi        RWO            Delete           Bound    workspace1/s2i-queue-pvc                                                                                                longhorn                43s
    pvc-39d799cf-6773-46f6-8f16-16778e008824   1Ti        RWO            Delete           Bound    workspace1/postgres-data-versioned-db-0                                                                                 longhorn                39s
    pvc-a16af508-e380-4243-944c-1473d2a6b711   10Mi       RWO            Delete           Bound    workspace1/persist-dir-secret-generator-0                                                                               longhorn                39s
    pvc-cb5c9ac7-0201-44be-aeb5-dec057db5870   1Ti        RWO            Delete           Bound    workspace1/s2i-git-server-repos-s2i-git-server-0                                                                        longhorn                37s
    pvc-fd0b773a-e785-4b5c-afc1-fcd98afcc56b   1Ti        RWO            Delete           Bound    workspace1/livelog-data-livelog-0                                                                                       longhorn                39s
    ```

    ```bash
# kubectl -n workspace1 get pods
NAME                                             READY   STATUS      RESTARTS   AGE
api-659964c875-4mmx8                             1/1     Running     2          91s
cron-6c49d46f96-r722z                            2/2     Running     0          93s
db-0                                             2/2     Running     0          93s
db-migrate-2.0.28-b66-7plvc                      0/1     Completed   0          93s
ds-cdh-client-6cf857bf5-vgtb8                    3/3     Running     0          92s
ds-operator-548fc76c87-874sl                     2/2     Running     0          91s
ds-reconciler-796b447c88-2shht                   2/2     Running     0          93s
ds-vfs-5d465b8df-x527k                           2/2     Running     0          92s
feature-flags-85b7884c59-j9ms7                   2/2     Running     0          93s
fluentd-forwarder-649dc8f55d-bsmmq               1/1     Running     0          93s
livelog-0                                        2/2     Running     0          93s
livelog-publisher-cnmkg                          2/2     Running     0          93s
livelog-publisher-l9q68                          2/2     Running     0          93s
mlx-workspace1-pod-evaluator-5b87db5cf6-rgct7    1/1     Running     0          93s
model-metrics-5f8fc48755-qnz26                   0/1     Running     4          93s
model-metrics-db-0                               1/1     Running     0          93s
model-proxy-59fbd8d695-q5zdv                     2/2     Running     0          91s
prometheus-postgres-exporter-69589689db-skjbl    1/1     Running     0          93s
runtime-addon-trigger-2.0.28-b66-5s55m           1/1     Running     0          93s
runtime-initial-repo-inserter-2.0.28-b66-s255d   1/1     Running     0          93s
runtime-manager-5cccc9cc5c-2dsgg                 2/2     Running     0          92s
s2i-builder-5b87d869d7-cf89p                     2/2     Running     0          91s
s2i-builder-5b87d869d7-mmfpm                     2/2     Running     0          91s
s2i-builder-5b87d869d7-ph49k                     2/2     Running     0          91s
s2i-client-869f5999dc-tktw5                      1/2     Running     0          92s
s2i-git-server-0                                 2/2     Running     0          93s
s2i-queue-0                                      2/2     Running     0          93s
s2i-server-86ccf5f57-7rx2f                       1/2     Running     0          92s
secret-generator-0                               2/2     Running     0          93s
tcp-ingress-controller-d9d4977bd-9sz48           1/2     Running     0          93s
usage-reporter-55f8cfdc97-hsqmp                  2/2     Running     0          93s
web-856f5d687c-flc4j                             1/2     Running     0          93s
web-856f5d687c-jtn2j                             1/2     Running     0          93s
web-856f5d687c-jxgxf                             1/2     Running     0          93s
    ```

