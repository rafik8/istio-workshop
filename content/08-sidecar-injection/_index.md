---
title: "Enable Istio for Hipster app"
chapter: true
weight: 8
---

# Enable Istio for Hispter app

We have seen in the previous chapter that we deployed the hipster commerce application and associate a DNS name for it.
Now, we will enable the service mesh for the application.
Istio suggests 3 methods enables service mesh:

**Automatic sidecar injection**: Automatic sidecar injection via a Mutating Admission Webhook. when a namespace is labelled `istio-injection=enabled`, every pod deployed to the namespace get a side car injected by `sidecar-injector` pod.

**Manual sidecar injection**:
Manual sidecar injection will add a sidecar to the deployment:
```
istioctl kube-inject -f deployment.yaml
```
**Single pod injection** (Experimental):
This is really useful when we want to enable mesh for only a specific service and start testing its compatibility with Istio.

```
istioctl experimental add-to-mesh service [flags]
```

In our workshop, we will use the namespace label method.


{{% children showhidden="false" %}}
