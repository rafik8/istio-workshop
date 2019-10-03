---
title: "Service Authorization using RBAC"
chapter: true
weight: 2
draft: true
---
# Service Authorization using RBAC

## Introduction

In large system, we need to isolate services and their interaction to avoid security issues and make sure that the flow is similar to what been designed.
## How it works

![Istio RBAC Authorization](/images/istio-rbac-auth.png)


## RBAC authorization in practice

1. Enable Istio RBAC for the `hipster-app` namespace.

```
apiVersion: "rbac.istio.io/v1alpha1"
kind: RbacConfig
metadata:
  name: default
spec:
  mode: 'ON_WITH_INCLUSION'
  inclusion:
    namespaces: ["hipster-app"]
```

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/istio-auth-rbac.yaml
```

1. Try to access to the hipster app:


![Istio RBAC Authorization](/images/hipster-rbac-fails.png)


2. Let's define a RBAC role:

```
apiVersion: "rbac.istio.io/v1alpha1"
kind: ServiceRole
metadata:
  name: viewer
  namespace: hipster-app
spec:
  rules:
  - services: ["*.hipster-app.svc.cluster.local"]
    methods: ["GET", "POST", "HEAD"]
---
apiVersion: rbac.istio.io/v1alpha1
kind: ServiceRoleBinding
metadata:
  name: bind-viewer
  namespace: hipster-app
spec:
  subjects:
   - user: "*"
  roleRef:
    kind: ServiceRole
    name: viewer
```

Apply the configuration:    


```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/istio-role-viewer.yaml
```

<!-- ## Validate policy:

Istio CLI provide a command to inspect and validate  RBAC policies:

```
istioctl experimental auth validate -f policy.yaml -->
```
