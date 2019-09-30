---
title: "Install Helm"
chapter: true
weight: 3
---
# Install Helm

[Helm](https://helm.sh/) is a package manager for Kubernetes that packages multiple Kubernetes resources into a single logical deployment unit called **Chart**.

Istio provides a helm chart for its installation and we will use this option to deploy Istio.

![Helm Logo](/images/helm-logo.png??width=250pc)

{{% notice note %}}
Istio operator will be the default and production recommended option.
{{% /notice%}}

## Install Helm CLI

Before we start configuring helm we’ll need firstly to install the command line tool that you will interact with the server side of Helm (Tiller).

1. Run the following command to install helm CLI:

```
cd $WORKSHOP_HOME

curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh

chmod +x get_helm.sh

./get_helm.sh

```

  {{% notice info %}}
  Once you install helm, the command will prompt you to run ‘helm init’. **Do not run ‘helm init’**. Follow the instructions to configure helm using **Kubernetes RBAC** and then install tiller as specified below If you accidentally run ‘helm init’, you can safely uninstall tiller by running ‘helm reset –force’
  {{% /notice%}}


1. Check helm is installed:

```
helm version
```

```
Client: &version.Version{SemVer:"v2.14.3", GitCommit:"0e7f3b6637f7af8fcfddb3d2941fcc7cbebb0085", GitTreeState:"clean"}
Error: could not find tiller
```

## Configure Helm access with RBAC

Helm relies on a service called **tiller** that requires special permission on the
kubernetes cluster, so we need to build a _**Service Account**_ for **tiller**
to use. We'll then apply this to the cluster.

1. Create a new service account manifest:

```
cat <<EoF > rbac.yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
EoF
```

2. Next apply the configuration:

```
kubectl apply -f rbac.yaml
```

```
serviceaccount/tiller created
clusterrolebinding.rbac.authorization.k8s.io/tiller created
```

3. Then we can install **tiller** using the **helm** CLI:

```
helm init --service-account tiller
```
This will install **tiller** into the cluster which gives it access to manage
resources in your cluster.

```
$HELM_HOME has been configured at /Users/user001/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.

Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
To prevent this, run `helm init` with the --tiller-tls-verify flag.
For more information on securing your installation see: https://docs.helm.sh/using_helm/#securing-your-helm-installation
```
4. Check helm installation:

```
helm version
```

```
Client: &version.Version{SemVer:"v2.14.3", GitCommit:"0e7f3b6637f7af8fcfddb3d2941fcc7cbebb0085", GitTreeState:"clean"}
Server: &version.Version{SemVer:"v2.14.3", GitCommit:"0e7f3b6637f7af8fcfddb3d2941fcc7cbebb0085", GitTreeState:"clean"}
```

```
kubectl get po -n kube-system

```

```
NAME                                                       READY   STATUS    RESTARTS   AGE
heapster-v1.6.1-84bdfd9b9d-72rzn                           3/3     Running   0          34h
kube-dns-6987857fdb-mskmd                                  4/4     Running   0          10h
kube-dns-6987857fdb-zktf7                                  4/4     Running   0          22h
kube-dns-autoscaler-bb58c6784-6kbsv                        1/1     Running   0          34h
kube-proxy-gke-istio-workshop-default-pool-2aaec539-8cbv   1/1     Running   0          22h
kube-proxy-gke-istio-workshop-default-pool-2aaec539-kc5r   1/1     Running   0          10h
kube-proxy-gke-istio-workshop-default-pool-2aaec539-rtqk   1/1     Running   0          34h
l7-default-backend-fd59995cd-7q95z                         1/1     Running   0          10h
metrics-server-v0.3.1-57c75779f-2j68r                      2/2     Running   0          34h
prometheus-to-sd-bxnkt                                     1/1     Running   0          34h
prometheus-to-sd-c6htk                                     1/1     Running   0          22h
prometheus-to-sd-g9frg                                     1/1     Running   0          10h
tiller-deploy-5d6cc99fc-5wdn8                              1/1     Running   0          2m34s
```
