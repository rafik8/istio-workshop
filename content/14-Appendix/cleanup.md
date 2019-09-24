---
title: "cleanup"
chapter: true
weight: 1
---



## Undeploy sample application:


**Skaffold**: you can run the following command to clean up the deployed resources.
```
skaffold delete
```

**kubectl**: you can run the following command to clean up the deployed resources.

```
kubectl delete -f ./release/kubernetes-manifests.yaml
```


## Uninstall Istio


```
helm delete istio --purge
helm delete istio-init --purge
helm delete istio-cni --purge

helm ls

kubectl -n istio-system delete job --all
kubectl delete namespace istio-system
kubectl delete -f istio-$ISTIO_VERSION/install/kubernetes/helm/istio-init/files
```

## Delete Kubernetes cluster
```shell
gcloud container clusters delete [CLUSTER_NAME]
```

```
The following clusters will be deleted.
 - [istio-workshop] in [europe-west1-b]

Do you want to continue (Y/n)?  Y

Deleting cluster istio-workshop...done.                                                                                                                                                                    
Deleted [https://container.googleapis.com/v1/projects/srecon19-workshop-250603/zones/europe-west1-b/clusters/istio-workshop].
```

## Delete Redis instance

```
gcloud redis instances delete redis-cart --region=$REGION_ID
```
