
# Compose on Kubernetes for Play with Kubernetes(PWK)

## Why Compose on Kubernetes? 

The Kubernetes API is really quite large. There are more than 50 first-class objects in the latest release, from Pods and Deployments to ValidatingWebhookConfiguration and ResourceQuota. This can lead to a verbosity in configuration, which then needs to be managed by you, the developer. 

The Kubernetes API is amazingly general purpose — it exposes low-level primitives for building the full range of distributed systems. Compose, meanwhile, isn't an API, but a high-level tool focused on developer productivity. That's why combining them together makes sense. For the common case of a set of interconnected web services, Compose provides an abstraction that simplifies Kubernetes configuration. For everything else, you can still drop down to the raw Kubernetes API primitives.

Now you can use Swarm CLI to manage Kubernetes Cluster in lot easier way. 

First, we need to install the Compose on Kubernetes controller into your Kubernetes cluster. This controller uses the standard Kubernetes extension points to introduce the "Stack" to the Kubernetes API. You can use any Kubernetes cluster you like, but if you don't already have one available then remember that Docker Desktop comes with Kubernetes and the Compose controller built-in, and enabling it is as simple as ticking a box in the settings.

Kops, short for Kubernetes Operations, is a set of tools for installing, operating, and deleting Kubernetes clusters in the cloud. A rolling upgrade of an older version of Kubernetes to a new version can also be performed. It also manages the cluster add-ons. After the cluster is created, the usual kubectl CLI can be used to manage resources in the cluster.

## Pre-requisite

- Go to https://labs.play-with-k8s.com
- Create your 1st Instance
- Clone the Repository and run this script on your 1st instance

```
git clone https://github.com/collabnix/compose-on-kubernetes
cd compose-on-kubernetes/scripts/pwk/
sh bootstrap-pwk.sh
```

- Copy the ```kubeadm join``` command and paste it on all the rest of instances

## Setting up 5-Node Kubernetes Cluster

```
chmod +x prepare-pwk.sh
sh prepare-pwk.sh
```

```
[node1 pwk]$ sh prepare-pwk.sh
Creating Compose Namespace...
namespace/compose created
Installing Helm...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 21.6M  100 21.6M    0     0  25.3M      0 --:--:-- --:--:-- --:--:-- 25.4M
Preparing Helm
linux-amd64/
linux-amd64/tiller
linux-amd64/helm
linux-amd64/LICENSE
linux-amd64/README.md
Creating tiller under kube-system namespace...
serviceaccount/tiller created
clusterrolebinding.rbac.authorization.k8s.io/tiller-cluster-rule created
Creating /root/.helm
Creating /root/.helm/repository
Creating /root/.helm/repository/cache
Creating /root/.helm/repository/local
Creating /root/.helm/plugins
Creating /root/.helm/starters
Creating /root/.helm/cache/archive
Creating /root/.helm/repository/repositories.yaml
Adding stable repo with URL: https://kubernetes-charts.storage.googleapis.com
Adding local repo with URL: http://127.0.0.1:8879/charts
$HELM_HOME has been configured at /root/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.

Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
To prevent this, run `helm init` with the --tiller-tls-verify flag.
For more information on securing your installation see: https://docs.helm.sh/using_helm/#securing-your-helm-installation
Happy Helming!
 Tiller is still coming up...Please Wait
NAME                             READY     STATUS    RESTARTS   AGE
coredns-78fcdf6894-699fx         1/1       Running   0          5m
coredns-78fcdf6894-trslx         1/1       Running   0          5m
etcd-node1                       1/1       Running   0          4m
kube-apiserver-node1             1/1       Running   0          4m
kube-controller-manager-node1    1/1       Running   0          4m
kube-proxy-5gskp                 1/1       Running   0          4m
kube-proxy-5hbkb                 1/1       Running   0          5m
kube-proxy-lcsnz                 1/1       Running   0          4m
kube-scheduler-node1             1/1       Running   0          4m
tiller-deploy-85744d9bfb-bjw2f   0/1       Running   0          15s
weave-net-9vt2s                  2/2       Running   1          4m
weave-net-k87d7                  2/2       Running   0          5m
weave-net-nmmt5                  2/2       Running   0          4m
NAME:   etcd-operator
LAST DEPLOYED: Mon Jan 21 14:27:50 2019
NAMESPACE: compose
STATUS: DEPLOYED

RESOURCES:
==> v1/ServiceAccount
NAME                                               SECRETS  AGE
etcd-operator-etcd-operator-etcd-backup-operator   1        4s
etcd-operator-etcd-operator-etcd-operator          1        4s
etcd-operator-etcd-operator-etcd-restore-operator  1        4s

==> v1beta1/ClusterRole
NAME                                       AGE
etcd-operator-etcd-operator-etcd-operator  4s

==> v1beta1/ClusterRoleBinding
NAME                                               AGE
etcd-operator-etcd-operator-etcd-backup-operator   4s
etcd-operator-etcd-operator-etcd-operator          3s
etcd-operator-etcd-operator-etcd-restore-operator  3s

==> v1/Service
NAME                   TYPE       CLUSTER-IP    EXTERNAL-IP  PORT(S)    AGE
etcd-restore-operator  ClusterIP  10.108.89.92  <none>       19999/TCP  3s

==> v1beta2/Deployment
NAME                                               DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
etcd-operator-etcd-operator-etcd-backup-operator   1        1        1           0          3s
etcd-operator-etcd-operator-etcd-operator          1        1        1           0          3s
etcd-operator-etcd-operator-etcd-restore-operator  1        1        1           0          3s

==> v1/Pod(related)
NAME                                                             READY  STATUS             RESTARTS  AGE
etcd-operator-etcd-operator-etcd-backup-operator-56fd448cd897mk  0/1    ContainerCreating  0      2s
etcd-operator-etcd-operator-etcd-operator-c5b8b8f74-pttr2        0/1    ContainerCreating  0      2s
etcd-operator-etcd-operator-etcd-restore-operator-58587cdc9g4br  0/1    ContainerCreating  0      2s


NOTES:
1. etcd-operator deployed.
  If you would like to deploy an etcd-cluster set cluster.enabled to true in values.yaml
  Check the etcd-operator logs
    export POD=$(kubectl get pods -l app=etcd-operator-etcd-operator-etcd-operator --namespacecompose --output name)
    kubectl logs $POD --namespace=compose
Loaded plugins: fastestmirror, ovl
base                                                                    | 3.6 kB  00:00:00
docker-ce-stable                                                        | 3.5 kB  00:00:00
extras                                                                  | 3.4 kB  00:00:00
kubernetes/signature                                                    |  454 B  00:00:00
kubernetes/signature                                                    | 1.4 kB  00:00:10 !!!
updates                                                                 | 3.4 kB  00:00:00
(1/7): base/7/x86_64/group_gz                                           | 166 kB  00:00:00
(2/7): extras/7/x86_64/primary_db                                       | 156 kB  00:00:00
(3/7): base/7/x86_64/primary_db                                         | 6.0 MB  00:00:00
(4/7): updates/7/x86_64/primary_db                                      | 1.3 MB  00:00:00
(5/7): docker-ce-stable/x86_64/primary_db                               |  20 kB  00:00:00
(6/7): docker-ce-stable/x86_64/updateinfo                               |   55 B  00:00:01
(7/7): kubernetes/primary                                               |  42 kB  00:00:01
Determining fastest mirrors
 * base: mirror.nl.datapacket.com
 * extras: mirror.nl.datapacket.com
 * updates: mirror.denit.net
kubernetes                                                                             305/305
Resolving Dependencies
--> Running transaction check
---> Package wget.x86_64 0:1.14-18.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===============================================================================================
 Package            Arch                 Version                      Repository          Size
===============================================================================================
Installing:
 wget               x86_64               1.14-18.el7                  base               547 k

Transaction Summary
===============================================================================================
Install  1 Package

Total download size: 547 k
Installed size: 2.0 M
Downloading packages:
wget-1.14-18.el7.x86_64.rpm                                             | 547 kB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : wget-1.14-18.el7.x86_64                                                     1/1
install-info: No such file or directory for /usr/share/info/wget.info.gz
  Verifying  : wget-1.14-18.el7.x86_64                                                     1/1

Installed:
  wget.x86_64 0:1.14-18.el7

Complete!
$HELM_HOME has been configured at /root/.helm.

Tiller (the Helm server-side component) has been upgraded to the current version.
Happy Helming!
etcdcluster.etcd.database.coreos.com/compose-etcd created
NAME                             READY     STATUS    RESTARTS   AGE
coredns-78fcdf6894-699fx         1/1       Running   0          6m
coredns-78fcdf6894-trslx         1/1       Running   0          6m
etcd-node1                       1/1       Running   0          5m
kube-apiserver-node1             1/1       Running   0          6m
kube-controller-manager-node1    1/1       Running   0          5m
kube-proxy-5gskp                 1/1       Running   0          6m
kube-proxy-5hbkb                 1/1       Running   0          6m
kube-proxy-lcsnz                 1/1       Running   0          5m
kube-scheduler-node1             1/1       Running   0          5m
tiller-deploy-85744d9bfb-bjw2f   1/1       Running   0          1m
weave-net-9vt2s                  2/2       Running   1          6m
weave-net-k87d7                  2/2       Running   0          6m
weave-net-nmmt5                  2/2       Running   0          5m
--2019-01-21 14:28:49--  https://github.com/docker/compose-on-kubernetes/releases/download/v0.4.18/installer-linux
Resolving github.com (github.com)... 140.82.118.3, 140.82.118.4
Connecting to github.com (github.com)|140.82.118.3|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://github-production-release-asset-2e65be.s3.amazonaws.com/158560458/e9a86500-15b2-11e9-8620-1eec5bf160e3?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20190121%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20190121T142850Z&X-Amz-Expires=300&X-Amz-Signature=bd4020beb0f68210e2a3cfa8ca8166dddcf1d1e4868737eb9ad83363cd39c660&X-Amz-SignedHeaders=host&actor_id=0&response-content-disposition=attachment%3B%20filename%3Dinstaller-linux&response-content-type=application%2Foctet-stream [following]
--2019-01-21 14:28:50--  https://github-production-release-asset-2e65be.s3.amazonaws.com/158560458/e9a86500-15b2-11e9-8620-1eec5bf160e3?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20190121%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20190121T142850Z&X-Amz-Expires=300&X-Amz-Signature=bd4020beb0f68210e2a3cfa8ca8166dddcf1d1e4868737eb9ad83363cd39c660&X-Amz-SignedHeaders=host&actor_id=0&response-content-disposition=attachment%3B%20filename%3Dinstaller-linux&response-content-type=application%2Foctet-stream
Resolving github-production-release-asset-2e65be.s3.amazonaws.com (github-production-release-asset-2e65be.s3.amazonaws.com)... 52.216.161.163
Connecting to github-production-release-asset-2e65be.s3.amazonaws.com (github-production-release-asset-2e65be.s3.amazonaws.com)|52.216.161.163|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 28376064 (27M) [application/octet-stream]
Saving to: 'installer-linux'

100%[=====================================================>] 28,376,064  15.9MB/s   in 1.7s

2019-01-21 14:28:52 (15.9 MB/s) - 'installer-linux' saved [28376064/28376064]

INFO[0000] Checking installation state
INFO[0000] Install image with tag "v0.4.18" in namespace "compose"
INFO[0000] Api server: image: "docker/kube-compose-api-server:v0.4.18", pullPolicy: "Always"
INFO[0001] Controller: image: "docker/kube-compose-controller:v0.4.18", pullPolicy: "Always"
failed to find a Stack API version
error: the server doesn't have a resource type "stacks"
[node1 pwk]$ sh prepare-pwk.sh
Creating Compose Namespace...
Error from server (AlreadyExists): namespaces "compose" already exists
Installing Helm...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 21.6M  100 21.6M    0     0  14.9M      0  0:00:01  0:00:01 --:--:-- 14.9M
Preparing Helm
linux-amd64/
linux-amd64/tiller
linux-amd64/helm
linux-amd64/LICENSE
linux-amd64/README.md
Creating tiller under kube-system namespace...
Error from server (AlreadyExists): serviceaccounts "tiller" already exists
Error from server (AlreadyExists): clusterrolebindings.rbac.authorization.k8s.io "tiller-cluster-rule" already exists
$HELM_HOME has been configured at /root/.helm.

Tiller (the Helm server-side component) has been upgraded to the current version.
Happy Helming!
 Tiller is still coming up...Please Wait
NAME                             READY     STATUS    RESTARTS   AGE
coredns-78fcdf6894-699fx         1/1       Running   0          7m
coredns-78fcdf6894-trslx         1/1       Running   0          7m
etcd-node1                       1/1       Running   0          6m
kube-apiserver-node1             1/1       Running   0          6m
kube-controller-manager-node1    1/1       Running   0          6m
kube-proxy-5gskp                 1/1       Running   0          6m
kube-proxy-5hbkb                 1/1       Running   0          7m
kube-proxy-lcsnz                 1/1       Running   0          6m
kube-scheduler-node1             1/1       Running   0          6m
tiller-deploy-85744d9bfb-bjw2f   1/1       Running   0          2m
weave-net-9vt2s                  2/2       Running   1          6m
weave-net-k87d7                  2/2       Running   0          7m
weave-net-nmmt5                  2/2       Running   0          6m
NAME            REVISION        UPDATED                         STATUS          CHART        APP VERSION     NAMESPACE
etcd-operator   1               Mon Jan 21 14:27:50 2019        DEPLOYED        etcd-operator-0.8.3    0.9.3           compose
Error: a release named etcd-operator already exists.
Run: helm ls --all etcd-operator; to check the status of the release
Or run: helm del --purge etcd-operator; to delete it
Loaded plugins: fastestmirror, ovl
Loading mirror speeds from cached hostfile
 * base: mirror.nl.datapacket.com
 * extras: mirror.nl.datapacket.com
 * updates: mirror.denit.net
Package wget-1.14-18.el7.x86_64 already installed and latest version
Nothing to do
$HELM_HOME has been configured at /root/.helm.

Tiller (the Helm server-side component) has been upgraded to the current version.
Happy Helming!
etcdcluster.etcd.database.coreos.com/compose-etcd unchanged
NAME                             READY     STATUS    RESTARTS   AGE
coredns-78fcdf6894-699fx         1/1       Running   0          7m
coredns-78fcdf6894-trslx         1/1       Running   0          7m
etcd-node1                       1/1       Running   0          6m
kube-apiserver-node1             1/1       Running   0          7m
kube-controller-manager-node1    1/1       Running   0          7m
kube-proxy-5gskp                 1/1       Running   0          7m
kube-proxy-5hbkb                 1/1       Running   0          7m
kube-proxy-lcsnz                 1/1       Running   0          6m
kube-scheduler-node1             1/1       Running   0          6m
tiller-deploy-85744d9bfb-bjw2f   1/1       Running   0          2m
weave-net-9vt2s                  2/2       Running   1          7m
weave-net-k87d7                  2/2       Running   0          7m
weave-net-nmmt5                  2/2       Running   0          6m
--2019-01-21 14:30:05--  https://github.com/docker/compose-on-kubernetes/releases/download/v0.4.18/installer-linux
Resolving github.com (github.com)... 140.82.118.3, 140.82.118.4
Connecting to github.com (github.com)|140.82.118.3|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://github-production-release-asset-2e65be.s3.amazonaws.com/158560458/e9a86500-15b2-11e9-8620-1eec5bf160e3?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20190121%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20190121T143006Z&X-Amz-Expires=300&X-Amz-Signature=53d5f390f91b968a53219512c18b696e1a085cbbd59cdb953ca95bea1aca4d60&X-Amz-SignedHeaders=host&actor_id=0&response-content-disposition=attachment%3B%20filename%3Dinstaller-linux&response-content-type=application%2Foctet-stream [following]
--2019-01-21 14:30:06--  https://github-production-release-asset-2e65be.s3.amazonaws.com/158560458/e9a86500-15b2-11e9-8620-1eec5bf160e3?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20190121%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20190121T143006Z&X-Amz-Expires=300&X-Amz-Signature=53d5f390f91b968a53219512c18b696e1a085cbbd59cdb953ca95bea1aca4d60&X-Amz-SignedHeaders=host&actor_id=0&response-content-disposition=attachment%3B%20filename%3Dinstaller-linux&response-content-type=application%2Foctet-stream
Resolving github-production-release-asset-2e65be.s3.amazonaws.com (github-production-release-asset-2e65be.s3.amazonaws.com)... 52.216.233.59
Connecting to github-production-release-asset-2e65be.s3.amazonaws.com (github-production-release-asset-2e65be.s3.amazonaws.com)|52.216.233.59|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 28376064 (27M) [application/octet-stream]
Saving to: 'installer-linux.1'

100%[=====================================================>] 28,376,064  16.3MB/s   in 1.7s

2019-01-21 14:30:08 (16.3 MB/s) - 'installer-linux.1' saved [28376064/28376064]

INFO[0000] Checking installation state
INFO[0001] Compose version v0.4.18 is already installed in namespace "compose" with the same settings
compose.docker.com/v1beta1
compose.docker.com/v1beta2
Waiting for the stack to be stable and running...
db1: Pending            [pod status: 0/2 ready, 2/2 pending, 0/2 failed]
web1: Pending           [pod status: 0/3 ready, 3/3 pending, 0/3 failed]






db1: Ready              [pod status: 1/2 ready, 1/2 pending, 0/2 failed]
web1: Ready             [pod status: 1/3 ready, 2/3 pending, 0/3 failed]

Stack hellostack is stable and running

NAME         SERVICES   PORTS        STATUS                            CREATED AT
hellostack   2          web1: 8082   Progressing (Stack is starting)   2019-01-21T14:30:10Z
```




