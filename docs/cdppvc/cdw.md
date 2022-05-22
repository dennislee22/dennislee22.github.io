---
layout: default
title: Cloudera Data Warehouse
parent: Data Services
grand_parent: CDP Private Cloud
nav_order: 3
---

# Cloudera Data Warehouse
{: .no_toc }

Blah

- TOC
{:toc}

---

    


## Hive Impersonation (doas)

1. This is a mandatory step to allow other user to impersonate `hive` user to access Hive tables. This is the Hive default and Ranger is the only supported and recommended security model. Ensure that Hive Impersonation is enabled as shown below.

    ![](../../assets/cdw/hiveimpersonation.png)  


#  ldapsearch  -H ldap://idm.cdpkvm.cldr:389 -D "uid=admin,cn=users,cn=accounts,dc=cdpkvm,dc=cldr" -w 'rootroot' -b "cn=users,cn=accounts,dc=cdpkvm,dc=cldr" '(&(uid=ldapuser1))' | grep -v "#"

dn: uid=ldapuser1,cn=users,cn=accounts,dc=cdpkvm,dc=cldr
displayName: ldapuser1 ldapuser1
uid: ldapuser1
krbCanonicalName: ldapuser1@CDPKVM.CLDR
objectClass: top
objectClass: person
objectClass: organizationalperson
objectClass: inetorgperson
objectClass: inetuser
objectClass: posixaccount
objectClass: krbprincipalaux
objectClass: krbticketpolicyaux
objectClass: ipaobject
objectClass: ipasshuser
objectClass: ipaSshGroupOfPubKeys
objectClass: mepOriginEntry
loginShell: /bin/sh
initials: ll
gecos: ldapuser1 ldapuser1
sn: ldapuser1
homeDirectory: /home/ldapuser1
mail: ldapuser1@cdpkvm.cldr
krbPrincipalName: ldapuser1@CDPKVM.CLDR
givenName: ldapuser1
cn: ldapuser1 ldapuser1
ipaUniqueID: 4a377c9c-d82b-11ec-995e-525400b4be20
uidNumber: 371000021
gidNumber: 371000021
krbLastPwdChange: 20220520105515Z
krbExtraData:: AAKTc4dia2FkbWluZEBDRFBLVk0uQ0xEUgA=
mepManagedEntry: cn=ldapuser1,cn=groups,cn=accounts,dc=cdpkvm,dc=cldr
memberOf: cn=ipausers,cn=groups,cn=accounts,dc=cdpkvm,dc=cldr
krbTicketFlags: 128
krbLoginFailedCount: 0
krbPasswordExpiration: 20220818105515Z

search: 2
result: 0 Success



#  ldapsearch  -H ldap://idm.cdpkvm.cldr:389 -D "uid=admin,cn=users,cn=accounts,dc=cdpkvm,dc=cldr" -w 'rootroot' -b "cn=groups,cn=accounts,dc=cdpkvm,dc=cldr" '(&(member=uid=ldapuser1,cn=users,cn=accounts,dc=cdpkvm,dc=cldr))' | grep -v "#"

dn: cn=ipausers,cn=groups,cn=accounts,dc=cdpkvm,dc=cldr
objectClass: top
objectClass: groupofnames
objectClass: nestedgroup
objectClass: ipausergroup
objectClass: ipaobject
description: Default group for all users
cn: ipausers
ipaUniqueID: 894cae12-bcd2-11ec-9ceb-525400b4be20
member: uid=cmadmin-97fd6767,cn=users,cn=accounts,dc=cdpkvm,dc=cldr
member: uid=ldapuser1,cn=users,cn=accounts,dc=cdpkvm,dc=cldr
member: uid=test,cn=users,cn=accounts,dc=cdpkvm,dc=cldr

search: 2
result: 0 Success



postgres=# \c metastoredb1
You are now connected to database "metastoredb1" as user "postgres".
metastoredb1=# \dt
                     List of relations
 Schema |             Name              | Type  |  Owner   
--------+-------------------------------+-------+----------
 public | AUX_TABLE                     | table | cdpadmin
 public | BUCKETING_COLS                | table | cdpadmin
 public | CDH_VERSION                   | table | cdpadmin
 public | CDS                           | table | cdpadmin
 public | COLUMNS_V2                    | table | cdpadmin
 public | COMPACTION_QUEUE              | table | cdpadmin
 public | COMPLETED_COMPACTIONS         | table | cdpadmin
 public | COMPLETED_TXN_COMPONENTS      | table | cdpadmin
 public | CTLGS                         | table | cdpadmin
 public | DATABASE_PARAMS               | table | cdpadmin
 public | DBS                           | table | cdpadmin
 public | DB_PRIVS                      | table | cdpadmin
 public | DELEGATION_TOKENS             | table | cdpadmin
 public | FUNCS                         | table | cdpadmin
 public | FUNC_RU                       | table | cdpadmin
 public | GLOBAL_PRIVS                  | table | cdpadmin
 public | HIVE_LOCKS                    | table | cdpadmin
 public | IDXS                          | table | cdpadmin
 public | INDEX_PARAMS                  | table | cdpadmin
 public | I_SCHEMA                      | table | cdpadmin
 public | KEY_CONSTRAINTS               | table | cdpadmin
 public | MASTER_KEYS                   | table | cdpadmin
 public | MATERIALIZATION_REBUILD_LOCKS | table | cdpadmin
 public | METASTORE_DB_PROPERTIES       | table | cdpadmin
 public | MIN_HISTORY_LEVEL             | table | cdpadmin
 public | MV_CREATION_METADATA          | table | cdpadmin
 public | MV_TABLES_USED                | table | cdpadmin
 public | NEXT_COMPACTION_QUEUE_ID      | table | cdpadmin
 public | NEXT_LOCK_ID                  | table | cdpadmin
 public | NEXT_TXN_ID                   | table | cdpadmin
 public | NEXT_WRITE_ID                 | table | cdpadmin
 public | NOTIFICATION_LOG              | table | cdpadmin
 public | NOTIFICATION_SEQUENCE         | table | cdpadmin
 public | NUCLEUS_TABLES                | table | cdpadmin
 public | PACKAGES                      | table | cdpadmin
 public | PARTITIONS                    | table | cdpadmin
 public | PARTITION_EVENTS              | table | cdpadmin
 public | PARTITION_KEYS                | table | cdpadmin
 public | PARTITION_KEY_VALS            | table | cdpadmin
 public | PARTITION_PARAMS              | table | cdpadmin


postgres=# \c huedb1
You are now connected to database "huedb1" as user "postgres".
dbhue=# \dt
                     List of relations
 Schema |              Name              | Type  |  Owner   
--------+--------------------------------+-------+----------
 public | auth_group                     | table | cdpadmin
 public | auth_group_permissions         | table | cdpadmin
 public | auth_permission                | table | cdpadmin
 public | auth_user                      | table | cdpadmin
 public | auth_user_groups               | table | cdpadmin
 public | auth_user_user_permissions     | table | cdpadmin
 public | authtoken_token                | table | cdpadmin
 public | axes_accessattempt             | table | cdpadmin
 public | axes_accesslog                 | table | cdpadmin
 public | beeswax_metainstall            | table | cdpadmin
 public | beeswax_queryhistory           | table | cdpadmin
 public | beeswax_savedquery             | table | cdpadmin
 public | beeswax_session                | table | cdpadmin
 public | defaultconfiguration_groups    | table | cdpadmin
 public | desktop_connector              | table | cdpadmin
 public | desktop_defaultconfiguration   | table | cdpadmin
 public | desktop_document               | table | cdpadmin
 public | desktop_document2              | table | cdpadmin
 public | desktop_document2_dependencies | table | cdpadmin
 public | desktop_document2permission    | table | cdpadmin
 public | desktop_document_tags          | table | cdpadmin
 public | desktop_documentpermission     | table | cdpadmin
 public | desktop_documenttag            | table | cdpadmin
 public | desktop_settings               | table | cdpadmin
 public | desktop_userpreferences        | table | cdpadmin
 public | django_content_type            | table | cdpadmin
 public | django_migrations              | table | cdpadmin
 public | django_session                 | table | cdpadmin
 public | django_site                    | table | cdpadmin
 public | documentpermission2_groups     | table | cdpadmin
 public | documentpermission2_users      | table | cdpadmin
 public | documentpermission_groups      | table | cdpadmin
 public | documentpermission_users       | table | cdpadmin
 public | useradmin_grouppermission      | table | cdpadmin
 public | useradmin_huepermission        | table | cdpadmin
 public | useradmin_ldapgroup            | table | cdpadmin
 public | useradmin_userprofile          | table | cdpadmin


# kubectl get ns
NAME                                     STATUS   AGE
cdp                                      Active   9h
cdp-env-1-85b71ddc-monitoring-platform   Active   117m
default                                  Active   9h
default-470e0dec-monitoring-platform     Active   9h
ecs-webhooks                             Active   9h
impala-1653181201-5gqh                   Active   46m
infra-prometheus                         Active   9h
kube-node-lease                          Active   9h
kube-public                              Active   9h
kube-system                              Active   9h
kubernetes-dashboard                     Active   9h
local-path-storage                       Active   9h
longhorn-system                          Active   9h
shared-services                          Active   116m
vault-system                             Active   9h
warehouse-1653181106-8vj6                Active   47m
warehouse-1653181127-2v5k                Active   47m
yunikorn                                 Active   9h

# kubectl -n warehouse-1653181106-8vj6  get pods
NAME                    READY   STATUS    RESTARTS   AGE
das-event-processor-0   1/1     Running   0          49m
metastore-0             1/1     Running   0          49m
metastore-1             1/1     Running   0          48m

# kubectl -n warehouse-1653181127-2v5k get pods
NAME                                     READY   STATUS      RESTARTS   AGE
das-event-processor-0                    1/1     Running     0          46m
metastore-0                              1/1     Running     0          46m
metastore-1                              1/1     Running     0          45m
metastore-ranger-repo-create-job-fsjbk   0/1     Completed   0          46m


# kubectl -n impala-1653181201-5gqh get pods
NAME                                 READY   STATUS    RESTARTS   AGE
catalogd-5cf658bb69-ksl7s            1/1     Running   0          44m
coordinator-0                        4/4     Running   0          44m
huebackend-0                         2/2     Running   0          44m
huefrontend-5949769cf-562r4          1/1     Running   0          44m
impala-autoscaler-578776d9c7-7fz5d   1/1     Running   0          44m
impala-executor-000-0                1/1     Running   0          9m55s
statestored-6499c77cfc-x2h58         1/1     Running   0          44m
usage-monitor-85d5f97cf5-lj54b       1/1     Running   0          44m

# kubectl -n impala-1653181201-5gqh get pvc
NAME                                         STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
scratch-cache-volume-coordinator-0           Bound    pvc-f13cc78e-4422-4688-914e-4ae273f631b0   100Gi      RWO            local-path     44m
scratch-cache-volume-impala-executor-000-0   Bound    pvc-71bc8ec2-93d9-4006-afe5-3bb0bfbb0df1   100Gi      RWO            local-path     44m

# kubectl get pv | grep impala
pvc-71bc8ec2-93d9-4006-afe5-3bb0bfbb0df1   100Gi      RWO            Delete           Bound    impala-1653181201-5gqh/scratch-cache-volume-impala-executor-000-0                                                       local-path              46m
pvc-f13cc78e-4422-4688-914e-4ae273f631b0   100Gi      RWO            Delete           Bound    impala-1653181201-5gqh/scratch-cache-volume-coordinator-0                                                               local-path              46m


# kubectl describe pv pvc-f13cc78e-4422-4688-914e-4ae273f631b0
Name:              pvc-f13cc78e-4422-4688-914e-4ae273f631b0
Labels:            <none>
Annotations:       pv.kubernetes.io/provisioned-by: rancher.io/local-path
Finalizers:        [kubernetes.io/pv-protection]
StorageClass:      local-path
Status:            Bound
Claim:             impala-1653181201-5gqh/scratch-cache-volume-coordinator-0
Reclaim Policy:    Delete
Access Modes:      RWO
VolumeMode:        Filesystem
Capacity:          100Gi
Node Affinity:     
  Required Terms:  
    Term 0:        kubernetes.io/hostname in [ecsworker2.cdpkvm.cldr]
Message:           
Source:
    Type:          HostPath (bare host directory volume)
    Path:          /localpath/local-storage/pvc-f13cc78e-4422-4688-914e-4ae273f631b0_impala-1653181201-5gqh_scratch-cache-volume-coordinator-0
    HostPathType:  DirectoryOrCreate
Events:            <none>


# kubectl -n impala-1653181201-5gqh describe pod coordinator-0  | grep -i Node:
Node:         ecsworker2.cdpkvm.cldr/10.15.4.171



in ecsworker2 node, check the contents of the localpath directory.

# tree /localpath
/localpath
`-- local-storage
    `-- pvc-f13cc78e-4422-4688-914e-4ae273f631b0_impala-1653181201-5gqh_scratch-cache-volume-coordinator-0
        |-- impala-cache-file-07482f078a6de140:f90999799fab14bf
        `-- impala-scratch

3 directories, 1 file

