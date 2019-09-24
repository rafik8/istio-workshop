---
title: "Enable Istio for Hipster app"
chapter: true
weight: 8
---

# Enable Istio for Hispter app

We have seen in the previous chapter that we deployed the hipster commerce application and associate a DNS name for it.
Now, we will enable the service mesh for the application.
Istio suggests 3 methods enables service mesh:

**Namespace wide**: we enable Istio the the specified namespace buy annotating the namespace. This is result to every pod deployed to the namespace get a side car injected by Istio `sidecar-injector`.

**Manual sidecar injection**:

```
istioctl kube-inject -f filename
```
**Single pod injection** (Istio 1.3):
This is really useful when you want to enable mesh for only for a specific service and start testing.


In our workshop, we will use the namespace wide method.


{{% children showhidden="false" %}}
