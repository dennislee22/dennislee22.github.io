---
layout: default
title: Setup ECS Environment
parent: "CDP PvC ECS: Day-2"
grand_parent: CDP Private Cloud
nav_order: 1
---

# Setup ECS Environment
{: .no_toc }

This article explains the step to setup the ECS environment so that user can administer the CDP PvC ECS cluster after successful installation.

---

1. Configure the ECS environment in the ECS master/server node.

    ```bash
    # cp /etc/rancher/rke2/rke2.yaml .kube/config
    # export PATH=$PATH:/var/lib/rancher/rke2/bin
    # kubectl get nodes
    NAME                     STATUS     ROLES                       AGE    VERSION
    ecsmaster1.cdpkvm.cldr   Ready      control-plane,etcd,master   120m   v1.21.8+rke2r2
    ecsworker1.cdpkvm.cldr   Ready      <none>                      117m   v1.21.8+rke2r2
    ecsworker2.cdpkvm.cldr   Ready      <none>                      117m   v1.21.8+rke2r2
    ```
    
2. Amend the `~/.bash_profile` login shell to include `export PATH=$PATH:/var/lib/rancher/rke2/bin` parameter to persist the environment setting.   


    ```bash
systemctl start cloudera-scm-server
systemctl enable cloudera-scm-server
tail -f /var/log/cloudera-scm-server/cloudera-scm-server.log

http://192.168.79.102:7180/cmf/login

systemctl restart cloudera-scm-server

keytool -import -alias postgres -file /root/db.crt -storetype JKS -keystore /var/lib/cloudera-scm-agent/agent-cert/cm-auto-global_truststore.jks

/var/lib/cloudera-scm-agent/agent-cert/cm-auto-global_truststore.jks
cat /etc/hadoop/conf/ssl-client.xml | grep ssl.client.truststore.password -A1
    <name>ssl.client.truststore.password</name>
    <value>ifnGu5DNun1iKqTE9mcJXTqkuTmtZntXbdTHa8oUygx</value>

keytool -list -v -keystore /var/lib/cloudera-scm-agent/agent-cert/cm-auto-global_truststore.jks


/opt/cloudera/cm/bin/gen_credentials_ipa.sh
kinit -k -t $CMF_KEYTAB_FILE $CMF_PRINCIPAL
 echo 'rootroot' | kinit admin
KEYTAB_OUT=$1
PRINCIPAL=$2
MAX_RENEW_LIFE=$3

if [[ $ERR -eq 0 ]]; then
  echo "Host $HOST exists"
else
  echo "Adding new host: $HOST"
  if [[ "$HOST" =~ \. ]]; then
  ipa host-add $HOST  --force --no-reverse
  else
  ipa host-add $HOST.cdpkvm.cldr  --force --no-reverse
  fi
  #ipa host-add $HOST --force --no-reverse
fi

# CM
ldap://dlee-ipa.cdpkvm.cldr
uid=admin,cn=users,cn=accounts,dc=cdpkvm,dc=cldr
(uid={0})
cn=users,cn=accounts,dc=cdpkvm,dc=cldr
(member={0})
cn=groups,cn=accounts,dc=cdpkvm,dc=cldr



(&(member={0})(objectClass=posixgroup))

# Console
ldap://dlee-ipa.cdpkvm.cldr
uid=admin,cn=users,cn=accounts,dc=cdpkvm,dc=cldr
cn=users,cn=accounts,dc=cdpkvm,dc=cldr
(&(uid={0})(objectClass=person))
cn=groups,cn=accounts,dc=cdpkvm,dc=cldr
(&(member={1})

SSL

openssl genrsa -des3 -out rootCA.key 4096
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.crt
openssl genrsa -out cdpkvm.key 2048

openssl req -new -sha256 -key cdpkvm.key -subj "/C=SG/ST=SG/O=Spore/CN=db.cdpkvm.cldr" -out cdpkvm.csr

openssl req -in cdpkvm.csr -noout -text
openssl x509 -req -in cdpkvm.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out cdpkvm.crt -days 500 -sha256

openssl verify -CAfile rootCA.crt cdpkvm.crt

cat rootCA.crt | openssl enc -A -base64

Updating miscellaneous cert goes into cdp-pvc-truststore CM of cdp namespace. 

#read
openssl x509 -text -noout -in domain.crt

openssl x509 -text -noout -in  /var/lib/cloudera-scm-server/certmanager/trust-store/cm-auto-in_cluster_ca_cert.pem

openssl crl2pkcs7 -nocrl -certfile /etc/pki/tls/certs/ca-bundle.crt | openssl pkcs7 -print_certs | grep subject

https://192.168.79.102:7183

kinit admin

AUTOTLS:

Ranger2022


### client
sed -i 's/enforcing/disabled/g' /etc/selinux/config
yum install krb5-workstation krb5-libs -y
yum install freeipa-client -y
yum install java-11-openjdk-devel -y
yum update -y;reboot


vi /etc/resolv.conf
vi /etc/hosts
vi /etc/sysconfig/network-scripts/ifcfg-eth0 
PEERDNS=no

echo never > /sys/kernel/mm/transparent_hugepage/defrag
ipa-client-install --mkhomedir

 echo 'rootroot' | kinit admin

￼
[root@ecsmaster1 ~]# oc -n cdp get configmap cdp-pvc-truststore
NAME                 DATA   AGE
cdp-pvc-truststore   3      6d5h

CDW
openssl s_client -connect "hs2-hive9.apps.ecs1.cdpkvm.cldr:443" -showcerts </dev/null 2>/dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > “ecsingress.crt"

keytool  -importcert -alias beeline -keystore /usr/lib/jvm/java-11-openjdk-11.0.15.0.9-2.el7_9.x86_64/lib/security/cacerts  -file  /root/ecsingress.crt

/usr/java/jdk1.8.0_232-cloudera/bin/keytool  -importcert -alias rootca -keystore /usr/java/jdk1.8.0_232-cloudera/jre/lib/security/cacerts -file ./routerca.crt

Enter keystore password:  changeit

keytool -list -v -keystore /usr/lib/jvm/java-11-openjdk-11.0.15.0.9-2.el7_9.x86_64/lib/security/cacerts | grep rootca

beeline -u "jdbc:hive2://hs2-hive9.apps.ecs1.cdpkvm.cldr/default;transportMode=http;httpPath=cliservice;ssl=true;retries=3;user=ldapuser1;password=ldapuser1"

# which beeline
/usr/bin/beeline

OCP4:
https://docs.openshift.com/container-platform/4.6/security/certificates/replacing-default-ingress-certificate.html

oc edit secret mysecret -n openshift-ingress -o yaml
cat asd | base64 -d > ingressca2.crt
keytool  -importcert -alias beeline -keystore /usr/lib/jvm/java-11-openjdk-11.0.15.0.9-2.el7_9.x86_64/lib/security/cacerts  -file  /root/ingressca2.crt
beeline -u "jdbc:hive2://hs2-hive.apps.ocp4.cdpkvm.cldr/default;transportMode=http;httpPath=cliservice;ssl=true;retries=1;user=ldapuser1;password=ldapuser1"

beeline -u "jdbc:hive2://hs2-hive.apps.ocp4.cdpkvm.cldr/db1;transportMode=http;httpPath=cliservice;ssl=true;retries=1;user=ldapuser1;password=ldapuser1" -f alltables.sql --hivevar LOCATION=/tmp/benchmark

cat /sys/fs/cgroup/pids/pids.max 
61440

oc debug
crictl ps

CDE
ipa-getkeytab -p ldapuser1@CDPKVM.CLDR -s idm.cdpkvm.cldr -k /root/ldapuser1.keytab --password -P

ECS:
./cdp-cde-utils.sh init-user-in-virtual-cluster -h qmmfqkr5.cde-7lkhvks4.apps.ecs1.cdpkvm.cldr -u ldapuser1 -p ldapuser1.principal -k ldapuser1.keytab 

user: ldapuser1
auth-pass-file: /root/credentials
vcluster-endpoint: https://snxg55zh.cde-gf6zglk2.apps.ecs1.cdpkvm.cldr/dex/api/v1
tls-insecure: true

./cde job create --type spark --application-file /root/wc.py --driver-cores 1 --driver-memory 4g --executor-cores 1 --executor-memory 2g --num-executors 3 --name testme --log-level DEBUG

OCP:
./cdp-cde-utils.sh init-virtual-cluster -h 8pzf9tsf.cde-vc4tzlpg.apps.apps.ocp4.cdpkvm.cldr -a

OZONE

kinit -kt hive.keytab hive/butility1.cdpkvm.cldr@CDPKVM.CLDR
ozone sh volume list
ozone sh bucket list /s3v
ozone sh key list s3v/cdplogs-env-1-03cf2a33
ozone sh key get s3v/cdplogs-env-1-03cf2a33/logs/env-env-1/file/2022070804_050005.gz 20.gz


[root@bmaster1 ~]# ozone sh bucket list /s3v | grep -A8 -B2 ocp1 
  "metadata" : { },
  "volumeName" : "s3v",
  "name" : "cdplogs-cdp-env-ocp1-55d251fa",
  "storageType" : "DISK",
  "versioning" : false,
  "usedBytes" : 388566,
  "usedNamespace" : 2,
  "creationTime" : "2022-08-26T07:02:46.736Z",
  "modificationTime" : "2022-08-26T07:02:46.736Z",
  "quotaInBytes" : -1,
  "quotaInNamespace" : -1

￼

# oc logs dex-app-sgqzs2ng-api-684cfbc4c6-5plpq
2022/07/24 08:51:06 DEBUG runs.go:328 Run context created: &{Job:0xc0005bc0e0 WorkDir:/app/dex/storage/run/1/workspace Overrides:<nil> Variables:map[] TgtSecretName:ldapuser1-krb5-secret ServiceAccountName: ResourceHelper:0xc00042ac40 VcInfo:map[airflowEnabled:true appFQDN:sgqzs2ng.cde-vc4tzlpg.apps.apps.ocp4.cdpkvm.cldr appID:dex-app-sgqzs2ng appName:vcluster1 atlasUIEndpoint:<nil> cdeConsoleURL:https://console-cdp.apps.ocp4.cdpkvm.cldr/dex cloudPlatform:OPENSHIFT clusterFQDN:service.cde-vc4tzlpg.apps.apps.ocp4.cdpkvm.cldr clusterID:cluster-vc4tzlpg datalakeFileSystems:<nil> defaultMemoryOverheadFactor:0.1 logLocation:s3a://cdplogs-cdp-env-1-7efb4f6a/logs nodeAllocatableCPUMilli:<nil> nodeAllocatableMemMiB:<nil> nodeCpus:<nil> nodeMemGiB:<nil> nonJVMMemoryOverheadFactor:0.4 ozoneS3Endpoint:https://bworker3.cdpkvm.cldr:9879 ozoneS3KeyID:hive/butility1.cdpkvm.cldr@CDPKVM.CLDR pipelinesEnabled:false quotaCpu:<nil> quotaMem:<nil> roleProxyDefaultRole:<nil> roleProxyEnabled:false safariEnabled:false smtpEnabled:false sparkVersion:3.2.0 spotComponents:<nil> spotEnabled:false tlsEnabled:<nil> version:1.15.1-b36 workspaceCreationStrategy:copy workspaceMaxHardLinks:175]} [requestId 21432b351b080a07a4f343c6785c7eaa] [job run 1]


CML

[root@ecsmaster1 ~]# oc -n ws1 get secret tls-ca-additional 
NAME                TYPE     DATA   AGE
tls-ca-additional   Opaque   1      41m

export host=ml-7d391d67-760.apps.apps.ocp4.cdpkvm.cldr
openssl req -new -newkey rsa:3072 -nodes -keyout ${host}.key -subj "/CN=${host}/OU=PS/O=bank/ST=SG/C=SG" -out ${host}.csr
openssl req -in ${host}.csr -text -verify
echo "[default]
subjectAltName = @alt_names

[alt_names]
DNS.1 = *.${host}
DNS.2 = ${host}" > cml.ext

openssl x509 -req -extfile cml.ext -days 365 -in ${host}.csr -CA /root/self-signed/rootCA.crt -CAkey /root/self-signed/rootCA.key -CAcreateserial -out ${host}.crt

openssl x509 -noout -text -in ${host}.crt

kubectl delete secret/cml-tls-secret -n ws1
kubectl create secret tls cml-tls-secret --cert=${host}.crt --key=${host}.key -o yaml --dry-run | kubectl -n ws1 create -f -

kubectl get configmap -o jsonpath='{.binaryData}' private-cloud-ca-certs-pem -n ws1

Add /etc/hosts
livelog.ml-7d391d67-760.apps.apps.ocp4.cdpkvm.cldr

In Mac: use keychain access to trust the ml-* cert

GPU ECS:
https://docs.nvidia.com/datacenter/tesla/tesla-installation-notes/index.html
https://www.nvidia.com/Download/index.aspx?lang=en-us
https://nvidia.github.io/nvidia-container-runtime/
modinfo nvidia
BASE_URL=https://us.download.nvidia.com/tesla
DRIVER_VERSION=515.65.01
curl -fSsl -O $BASE_URL/$DRIVER_VERSION/NVIDIA-Linux-x86_64-$DRIVER_VERSION.run
sh NVIDIA-Linux-x86_64-$DRIVER_VERSION.run

curl -s -L https://nvidia.github.io/nvidia-container-runtime/$(. /etc/os-release;echo $ID$VERSION_ID)/nvidia-container-runtime.repo | sudo tee /etc/yum.repos.d/nvidia-container-runtime.repo
sudo yum -y install nvidia-container-runtime

https://wandb.ai/wandb/common-ml-errors/reports/How-To-Use-GPU-with-PyTorch---VmlldzozMzAxMDk
import torch
torch.cuda.is_available()
torch.cuda.device_count()
torch.cuda.get_device_name(0)
X_train = torch.FloatTensor([0., 1., 2.])
X_train.is_cuda
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
print(device)
X_train = X_train.to(device)
X_train.is_cuda

https://www.analyticsvidhya.com/blog/2021/11/benchmarking-cpu-and-gpu-performance-with-tensorflow/

Tensorflow:
LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/cuda/lib:/usr/lib/hadoop/lib/native

import tensorflow as tf
physical_devices = tf.config.list_physical_devices()
for dev in physical_devices:
  print(dev)

Jupiter cannot spawn after Tensorflow is installed. Install Tensorflow after Jupiter is spawned is ok.

DOCKER build
https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/

[root@nexus ~]# more cudatoolkit_for_tf.Dockerfile
FROM docker.repository.cloudera.com/cloudera/cdsw/engine:16-cml-2022.01-2
USER root
ARG OS=ubuntu1804
ARG cudnn_version=8.1.0.77
ARG cuda_version=cuda11.2

RUN wget https://developer.download.nvidia.com/compute/cuda/repos/${OS}/x86_64/cuda-${OS}.pin 
RUN mv cuda-${OS}.pin /etc/apt/preferences.d/cuda-repository-pin-600
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/${OS}/x86_64/3bf863cc.pub
RUN add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/${OS}/x86_64/ /"
RUN apt-get update

RUN apt-get install -y cuda-toolkit-11-2
RUN apt-get install -y libcudnn8=${cudnn_version}-1+${cuda_version}
RUN apt-get install -y libcudnn8-dev=${cudnn_version}-1+${cuda_version}
RUN apt-get install -y python3
RUN apt-get install -y python3-pip
RUN python3 -m pip install tensorflow

docker build --network host -t ecsgpu.cdpkvm.cldr:5000/cloudera/cudatoolkit_tf:11.2 .  -f cudatoolkit_for_tf.Dockerfile
docker image push ecsgpu.cdpkvm.cldr:5000/cloudera/cudatoolkit_tf:11.2 

/usr/local/bin/jupyter

!pip3 install jupyterlab
!/home/cdsw/.local/bin/jupyter-lab --no-browser --ip=127.0.0.1 --port=${CDSW_APP_PORT} --NotebookApp.token= --NotebookApp.allow_remote_access=True --log-level=ERROR

￼

ECS

[root@ecsmaster1 ~]# oc -n ws1 get secret cdp-private-installer-docker-pull-secret -o yaml
apiVersion: v1
data:
  .dockerconfigjson: eyJhdXRocyI6eyJlY3NncHUuY2Rwa3ZtLmNsZHI6NTAwMCI6eyJ1c2VybmFtZSI6InJlZ2lzdHJ5LXVzZXIiLCJwYXNzd29yZCI6ImRoanp5M2gwNGpzeTZra2JpcWxlMWV5IiwiYXV0aCI6ImNtVm5hWE4wY25rdGRYTmxjanBrYUdwNmVUTm9NRFJxYzNrMmEydGlhWEZzWlRGbGVRPT0ifX19
kind: Secret
metadata:
  creationTimestamp: "2022-08-25T02:13:00Z"
  name: cdp-private-installer-docker-pull-secret
  namespace: ws1
  resourceVersion: "39866"
  uid: a48d3ae4-8028-41eb-8b13-c5bf51064b13
type: Opaque

[root@ecsmaster1 ~]# more zxc | base64 -d
{"auths":{"ecsgpu.cdpkvm.cldr:5000":{"username":"registry-user","password":"dhjzy3h04jsy6kkbiqle1ey","auth":"cmVnaXN0cnktdXNlcjpkaGp6eTNoMDRqc3k2a2tiaXFsZTFleQ=="}}}

/var/lib/rancher/rke2/bin/kubectl
/etc/rancher/node/password
cp /etc/rancher/rke2/rke2.yaml .kube/config

kubectl get all --all-namespaces

kubectl get pods -o=jsonpath='{range .items[*]}{.metadata.name}{"\t"}{.spec.containers[].resources.requests.cpu}{"\n"}{end}' -n ws1

kubectl get pods -A -o wide --field-selector spec.nodeName=node1
kubectl get pods --all-namespaces -o wide --field-selector spec.nodeName=ecsworker2.cdpkvm.cldr 

kubectl delete pod pod1 -n ns1 --grace-period=0 --force
kubectl get volumeattachment
kubectl uncordon node1
kubectl get csr vault-csr -o 'jsonpath={.status.certificate}'
kubectl delete pod <pod-name> --grace-period=0 --force

kubectl get pods --all-namespaces -o wide --field-selector spec.nodeName=dlee-ecs-svr1.tech.dlee | awk '{ print $1 }'

kubectl get pods -A -o wide

kubectl get all -A

kubectl get ingress -A
kubectl get pods -n kube-system | grep ingress

# check ldap login failure
kubectl -n cdp logs cdp-release-thunderhead-consoleauthenticationcdp-96b8597ffjbkbr -c thunderhead-consoleauthenticationcdp

# check env registration
kubectl -n cdp logs -f cdp-release-thunderhead-environment-649b8547f9-fhfvb  -c thunderhead-environment

kubectl label nodes ecsworker1.cdpkvm.cldr type=node1
kubectl label nodes ecsworker1.cdpkvm.cldr type-

kubectl -n vault-system patch deployment.apps/vault-exporter -p '{"spec": {"template": {"spec": {"nodeSelector": {"kubernetes.io/hostname": "ecsworker2.cdpkvm.cldr"}}}}}'

kubectl -n impala-1654745792-8fkn patch deployment.apps/catalogd  -p '{"spec": {"template": {"spec": {"nodeSelector": {"kubernetes.io/hostname": "ecsworker1.cdpkvm.cldr"}}}}}'

kubectl -n impala-1654745792-8fkn patch statefulset.apps/huebackend  -p '{"spec": {"template": {"spec": {"nodeSelector": {"kubernetes.io/hostname": "ecsworker3.cdpkvm.cldr"}}}}}'

kubectl patch node ecsworker2.cdpkvm.cldr  -p '{"spec":{"unschedulable":true}}'

kubectl get secrets -o jsonpath="{.items[?(@.metadata.annotations['kubernetes\.io/service-account\.name']=='default')].data.token}"|base64 --decode

kubectl -n longhorn-system get secret basic-auth -o json | jq '.data | map_values(@base64d)'

kubectl -n longhorn-system describe ingress longhorn-ingress

kubectl get clusterrolebinding admin-user -n kube-system -o yaml

kubectl get serviceaccount -A | grep admin

kubectl delete ns dleews1  --force

kubectl delete all --all -n warehouse

kubectl get event --namespace abc-namespace --field-selector involvedObject.name=my-pod-zl6m6

kubectl -n cdp exec -ti  pod -- /bin/bash

kubectl cp compute-1654155938-84qt/query-executor-0-0:/mnt/config/current/hive ./hive

kubectl top nodes

kubectl -n longhorn-system describe configmap/longhorn-default-setting

kubectl exec -it nfs-server bash -c "echo 'Hello NFS World' > /exports/data-0001/index.html"  

kubectl run -it --rm --restart=Never busybox --image=busybox:1.28 -- nslookup dlee-cm.cdpkvm.cldr

kubectl -n kube-system exec rke2-coredns-rke2-coredns-5679c85bbb-9frfj  cat /etc/resolv.conf

kubectl -n ws1 get pvc projects-pvc -o yaml  > asd
kubectl -n ws1 delete pvc projects-pvc --force

kubectl exec -it cdp-embedded-db-0 -n cdp -- bash
psql -P pager=off -d db-env
select * from configs where attr like '%ozone%';

find / | grep registries.yaml
/run/cloudera-scm-agent/process/1546338952-ecs-ECS_AGENT/registries.yaml
cat /run/cloudera-scm-agent/process/1546338952-ecs-ECS_AGENT/registries.yaml
curl -s --insecure -u registry-user:vqc434aqyer8ajd17d3dq36 https://ecsworker2.cdpkvm.cldr:5000/v2/_catalog | jq

### cde
kubectl -n cdp logs -f cdp-release-dex-cp-678c8bc5b7-z8llx  -c dex-cp

# ozone
cd /opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p1000.24102687/lib/hadoop-ozone/bin
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.14.1.1-1.el7_9.x86_64
export OZONE_CONF_DIR=/etc/ozone/conf.cloudera.ozone
kinit ldap3
./ozone sh volume create --user=ldap3 o3://ozonesvc1/vol1
./ozone sh volume info o3://ozonesvc1/vol1
./ozone sh bucket create o3://ozonesvc1/vol1/bucket1
./ozone sh volume list o3://ozonesvc1



##### check dwx creation
kubectl -n cdp logs -f cdp-release-dwx-server-5c48b6d4-crbpn  -c dwx-server


USER=cm-longhorn; PASSWORD=cm-longhorn; echo "${USER}:$(openssl passwd -stdin -apr1 <<< ${PASSWORD})" > asd
kubectl -n longhorn-system create secret generic basic-authasd --from-file=asd --insecure-skip-tls-verify
kubectl -n longhorn-system edit secret basic-auth
auth: Y20tbG9uZ2hvcm46JGFwcjEkcU54WWVqRy4kTnU2ci4ubUdJQnJmMml0bG40ZkJOMAo=

# kubectl get configmap -n longhorn-system
NAME                       DATA   AGE
chart-content-longhorn     1      14h
chart-values-longhorn      0      14h
kube-root-ca.crt           1      14h
longhorn-default-setting   1      14h
longhorn-storageclass      1      14h


Kubectl -n longhorn-system logs -f longhorn-manager-gmnm4  | grep pvc-05eaf851-83c1-4647-819d-a8f11a5c3eca

Openshift
kubectl run mycurlpod --image=curlimages/curl -i --tty -- sh

oc get pods -n grafana -o name

oc delete pods -l app=prometheus

oc adm policy add-cluster-role-to-user cluster-monitoring-view -z grafana-serviceaccount
oc serviceaccounts get-token grafana-serviceaccount -n my-grafana

oc get secret prometheus-k8s-htpasswd -o jsonpath='{.data.auth}' | base64 -d > /tmp/htpasswd-tmp
htpasswd -s -b  /tmp/htpasswd-tmp grafana-user mysupersecretpasswd
oc patch secret prometheus-k8s-htpasswd -p "{\"data\":{\"auth\":\"$(base64 -w0 /tmp/htpasswd-tmp)\"}}"

oc exec monitoring-prometheus-server-684cbd49fd-n96jb -- printenv
oc logs -f pod -c container --tail=5

oc exec mypod -ti ruby-container -- bash
oc rsh pod

oc get secret/kubeadmin -n kube-system -o yaml

oc adm top pods
oc adm top node --selector=''
oc adm top nodes

oc adm policy add-cluster-role-to-user cluster-monitoring-view -z grafana-serviceaccount
oc serviceaccounts get-token grafana-serviceaccount -n my-grafana

oc rsync <pod-name>:/remote/dir/filename ./local/dir 
oc exec -it nfspod -- df -h /var/www/html

oc get pod --all-namespaces --field-selector=spec.nodeName=<nodename>

oc label node <node-name> node-role.kubernetes.io/infra=

oc describe rolebinding.rbac

oc describe ingresscontrollers/default -n openshift-ingress-operator

oc get networkpolicy -n cdp1234-vault

oc describe network.config/cluster

oc describe project.config.openshift.io/cluster (find out existence of default project template)

oc debug node/nodename

oc label namespace ws1 monitoring-platform-access=true

cat << EOF| oc create -f -
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: allow-userws
spec:
  ingress:
  - from:
    - namespaceSelector: 
        matchLabels:
          monitoring-platform-access: "true"  
    - podSelector: {}
  podSelector: {}
  policyTypes:
  - Ingress
EOF


kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: allow-port80
spec:
  ingress:
    - ports:
        - port: 80
      from: []
    - podSelector: {}
  podSelector: {}
  policyTypes:
  - Ingress

oc login https://10.15.4.250:6443 -u dennislee -p "7vx&vq\#BGMrB1V8"


#tls
oc create configmap custom-ca --from-file=ca-bundle.crt=/root/self-signed/rootCA.crt -n openshift-config
oc patch proxy/cluster --type=merge --patch='{"spec":{"trustedCA":{"name":"custom-ca"}}}'
oc patch ingresscontroller.operator default --type=merge -p '{"spec":{"defaultCertificate": {"name": "mysecret"}}}' -n openshift-ingress-operator


Linux

tcpdump -w output.pcap port 67 or port 68 or port 69 or port 4011

netstat -ltnp | grep -w '7182'
find / -name postgresql.conf 2>/dev/null

ip -s link show eth0

#self-sign
openssl genrsa -aes256 -out ca-key.pem 4096

curl localhost:8080 --connect-timeout 10 --max-time 10 -w "\n ##############\n %{url_effective} (%{remote_ip})\n ##############\n code: %{http_code}\n --- \n namelookup: %{time_namelookup}\n connect: %{time_connect}\n appconnect: %{time_appconnect}\n pretransfer: %{time_pretransfer}\n redirect: %{time_redirect}\n starttrans: %{time_starttransfer}\n ---\n total: %{time_total}\n\n" 


openssl base64 -d YWRtaW4=
admin

COUNT=0;while true; do echo $COUNT; curl http://nginx.apps.ecs1.cdpkvm.cldr; let COUNT=COUNT+1; done

nc -zv 172.30.9.106 80

lsblk -d -e 7 -o NAME,ROTA,DISC-MAX,MODEL

openssl x509 -text -noout -in domain.crt

openssl x509 -enddate -noout -in /opt/cloudera/security/pki/$(hostname -f)-server.cert.pem

ldapsearch  -H ldap://idm.cdpkvm.cldr:389 -D "uid=admin,cn=users,cn=accounts,dc=cdpkvm,dc=cldr" -w 'rootroot' -b "cn=groups,cn=accounts,dc=cdpkvm,dc=cldr" -s sub "(objectClass=posixgroup)"

ldapsearch  -H ldap://idm.cdpkvm.cldr:389 -D "uid=admin,cn=users,cn=accounts,dc=cdpkvm,dc=cldr" -w 'rootroot' -b "cn=users,cn=accounts,dc=cdpkvm,dc=cldr" '(&(uid=ldap3))'

ldapsearch -x -H ldaps://idm.cdpkvm.cldr:636 -D "uid=admin,cn=users,cn=accounts,dc=cdpkvm,dc=cldr" -w 'rootroot' -b "cn=users,cn=accounts,dc=cdpkvm,dc=cldr" '(&(uid=ldapuser1))'

ldapsearch  -H ldap://idm.cdpkvm.cldr:389 -D "uid=admin,cn=users,cn=accounts,dc=cdpkvm,dc=cldr" -w 'rootroot' -b "cn=users,cn=accounts,dc=cdpkvm,dc=cldr" '(&(uid=ldap3)(memberof=cn=ipausers,cn=groups,cn=accounts,dc=cdpkvm,dc=cldr))'

(&(uid={0}) ->
ldapsearch  -H ldap://idm.cdpkvm.cldr:389 -D "uid=admin,cn=users,cn=accounts,dc=cdpkvm,dc=cldr" -w 'rootroot' -b "cn=users,cn=accounts,dc=cdpkvm,dc=cldr" '(&(uid=ldapuser1))'

(&(member={1}) ->
 ldapsearch  -H ldap://idm.cdpkvm.cldr:389 -D "uid=admin,cn=users,cn=accounts,dc=cdpkvm,dc=cldr" -w 'rootroot' -b "cn=groups,cn=accounts,dc=cdpkvm,dc=cldr" '(&(member=uid=ldap3,cn=users,cn=accounts,dc=cdpkvm,dc=cldr))'


keytool -printcert -v -file jssecacerts.pem | less

yum install sysstat -y
yum install iotop -y "Percentage of CPU time during which I/O requests were issued to the device"
iostat -d -k -t -N -x 10 10

yum install epel-release -y
yum install iftop -y
iftop -t
ip -s link show eth0

dlee-ecs-svr1.tech.dlee, dlee-ecs-svr2.tech.dlee, dlee-ecs-svr3.tech.dlee, dlee-ecs-agent1.tech.dlee, dlee-ecs-agent2.tech.dlee


DSA-ws1-5890cd23a9fa4fb889d27828ceab49c5
http://172.30.9.106

lsof -i -P -n | grep LISTEN


cp nexus.crt /etc/pki/ca-trust/source/anchors/ 
update-ca-trust extract

systemctl restart cloudera-scm-supervisord.service

HADOOP

kinit admin
hdfs dfs -df -h
hdfs dfs -ls -R /warehouse/tablespace
hdfs dfs -ls /warehouse/
hdfs dfsadmin -report
hdfs dfs -cat /tmp/sparkinput.txt
hdfs dfs -copyFromLocal /root/asd /tmp/

kinit hive/dwx-env-gsrwxm@CDPKVM.CLDR -kt /mnt/config/current/hive
klist
hdfs dfs -ls /warehouse/

kinit -kt /run/cloudera-scm-agent/process/1546334839-hdfs-DATANODE/hdfs.keytab hdfs/bworker3.cdpkvm.cldr@CDPKVM.CLDR
hdfs dfs -ls /user

hdfs dfs -touchz /user/asd


https://github.com/cloudera/partner-engineering/archive/master.zip

yarn jar /opt/cloudera/parcels/CDH/jars/hadoop-mapreduce-client-jobclient-3.1.1.7.1.7.1000-141-tests.jar TestDFSIO -write -nrFiles 1000 -size 1GB


    ```