---
title: "Notes"
chapter: true
weight: 3
draft: true
---
### GKE
If you are running on a GKE cluster with RBAC enabled you need to grant a cluster role of cluster-admin to your Google account:

```
kubectl create clusterrolebinding cluster-admin-binding-$USER \
    --clusterrole=cluster-admin --user=$(gcloud config get-value account)
```
