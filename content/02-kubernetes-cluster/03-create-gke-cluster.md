---
title: "Create GKE Cluster"
chapter: true
weight: 3
---
# Create GKE Cluster
<!-- **Duration**: 5:00 -->

1. Enable the Kubernetes and Cloud DNS services for your project:

```shell
gcloud services enable container.googleapis.com
gcloud services enable dns.googleapis.com
```

1. Get the last GKE Kubernetes version:  

```
export K8S_VERSION=$(gcloud container get-server-config --zone=europe-west1-c --format=json | jq -r '.validMasterVersions[0]')
```

1. Create a cluster with 3 nodes using the latest Kubernetes version:

```
gcloud container clusters create istio-workshop \
--cluster-version=${K8S_VERSION} \
--zone=${ZONE_ID} \
--num-nodes=3 \
--machine-type=n1-highcpu-4 \
--preemptible \
--no-enable-cloud-logging \
--disk-size=50 \
--enable-autorepair \
--scopes=gke-default \
--enable-network-policy \
--enable-ip-alias
```

We create a cluster with 3 nodes of type `n1-highcpu-4` (vCPU: 4, RAM 3.6 GB) **preemptible VMs**.

```shell
Creating cluster istio-workshop in europe-west1-b... Cluster is being health-checked (master is healthy)...done.                                                                                           
Created [https://container.googleapis.com/v1/projects/srecon19-workshop-250603/zones/europe-west1-b/clusters/istio-workshop].
To inspect the contents of your cluster, go to: https://console.cloud.google.com/kubernetes/workload_/gcloud/europe-west1-b/istio-workshop?project=srecon19-workshop-250603
kubeconfig entry generated for istio-workshop.
NAME            LOCATION        MASTER_VERSION  MASTER_IP       MACHINE_TYPE  NODE_VERSION   NUM_NODES  STATUS
istio-workshop  europe-west1-b  1.13.7-gke.19   146.148.23.108  n1-highcpu-4  1.13.7-gke.19  3          RUNNING
```

{{% notice tip %}}
[preemptible VMs](https://cloud.google.com/preemptible-vms/) are up to 80% cheaper than regular instances and are terminated and replaced after a maximum of 24 hours.
{{% /notice%}}

1. Check that the cluster is accessible:

```
kubectl version
```
```shell
Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.6", GitCommit:"abdda3f9fefa29172298a2e42f5102e777a8ec25", GitTreeState:"clean", BuildDate:"2019-05-08T13:53:53Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"darwin/amd64"}
Server Version: version.Info{Major:"1", Minor:"13+", GitVersion:"v1.13.7-gke.19", GitCommit:"bebe882824db5431820e3d59851c8fb52cb41675", GitTreeState:"clean", BuildDate:"2019-07-26T00:09:47Z", GoVersion:"go1.11.5b4", Compiler:"gc", Platform:"linux/amd64"}
```

```
kubectl get nodes
```

```shell
NAME                                            STATUS   ROLES    AGE     VERSION
gke-istio-workshop-default-pool-2aaec539-8cbv   Ready    <none>   4m19s   v1.13.7-gke.19
gke-istio-workshop-default-pool-2aaec539-kc5r   Ready    <none>   4m18s   v1.13.7-gke.19
gke-istio-workshop-default-pool-2aaec539-rtqk   Ready    <none>   4m18s   v1.13.7-gke.19
```
