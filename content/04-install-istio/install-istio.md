---
title: "Install Istio"
chapter: true
weight: 3
---

# Verification

Begin the Istio pre-installation verification check by running:

```
istioctl verify-install
```

```
Checking the cluster to make sure it is ready for Istio installation...

#1. Kubernetes-api
-----------------------
Can initialize the Kubernetes client.
Can query the Kubernetes API Server.

#2. Kubernetes-version
-----------------------
Istio is compatible with Kubernetes: v1.13.7-gke.19.

#3. Istio-existence
-----------------------
Istio will be installed in the istio-system namespace.

#4. Kubernetes-setup
-----------------------
Can create necessary Kubernetes configurations: Namespace,ClusterRole,ClusterRoleBinding,CustomResourceDefinition,Role,ServiceAccount,Service,Deployments,ConfigMap.

#5. Sidecar-Injector
-----------------------
This Kubernetes cluster supports automatic sidecar injection. To enable automatic sidecar injection see https://istio.io/docs/setup/kubernetes/additional-setup/sidecar-injection/#deploying-an-app

-----------------------
Install Pre-Check passed! The cluster is ready for Istio installation.
```

# Install Istio

```
cd releases/istio-$ISTIO_VERSION
```
## Define service account for Tiller

As we will install Istio using helm, first create a service account for Tiller:
```
//kubectl apply -f install/kubernetes/helm/helm-service-account.yaml
```


## Istio namespace

Create a namespace for the istio-system components:

```
kubectl create namespace istio-system
```

### Install Istio CRDs
The [Custom Resource Definitions, also known as CRDs](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions) are API resources which allow you to define custom resources.
```
helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system
```

You can check the installation by running:

```
kubectl get crds | grep 'istio.io\|certmanager.k8s.io' | wc -l
```
This should return  23 CRDs as of Istio 1.3.0.

{{% notice note %}}
If cert-manager is enabled, then the CRD count will be 28 instead.
{{% /notice%}}

Run also `helm ls` and you sould get the status of `istio-init` chart to `DEPLOYED`:

```
helm ls
```

```
NAME      	REVISION	UPDATED                 	STATUS  	CHART           	APP VERSION	NAMESPACE   
istio-init	1       	Sat Aug 31 15:44:51 2019	DEPLOYED	istio-init-1.3.0	1.3.0      	istio-system
```


## Install Istio CNI

As discussed eralier, Istio CNI is responsible for ....

```
helm install install/kubernetes/helm/istio-cni --name istio-cni --namespace kube-system -f install/kubernetes/helm/istio-cni/values_gke.yaml
```

```
NAME:   istio-cni
LAST DEPLOYED: Sun Sep 15 02:16:45 2019
NAMESPACE: kube-system
STATUS: DEPLOYED

RESOURCES:
==> v1/ClusterRole
NAME       AGE
istio-cni  0s

==> v1/ClusterRoleBinding
NAME       AGE
istio-cni  0s

==> v1/ConfigMap
NAME              DATA  AGE
istio-cni-config  1     0s

==> v1/DaemonSet
NAME            DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR                AGE
istio-cni-node  3        3        0      3           0          beta.kubernetes.io/os=linux  0s

==> v1/Pod(related)
NAME                  READY  STATUS             RESTARTS  AGE
istio-cni-node-4m65l  0/1    ContainerCreating  0         0s
istio-cni-node-bdtbz  0/1    ContainerCreating  0         0s
istio-cni-node-mp7rh  0/1    ContainerCreating  0         0s

==> v1/ServiceAccount
NAME       SECRETS  AGE
istio-cni  1        0s
```

### Install Istio
The last step installs Istio's core components:

```
helm install install/kubernetes/helm/istio --name istio --namespace istio-system --set tracing.enabled=true  --set grafana.enabled=true --set kiali.enabled=true --set istio_cni.enabled=true
```

or

```
helm install install/kubernetes/helm/istio --name istio --namespace istio-system  -f ../istio-srecon-values.yaml
```

- `istio_cni.enabled=true`: we are enabling istio cni to handle iptables configuration instead of Istio init.

- `grafana.enabled=true`: enable grafana dashboard.
