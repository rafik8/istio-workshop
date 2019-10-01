---
title: "Istio debugging tools"
chapter: true
weight: 12
---

# Debugging with Istio

1. check configuration:
```
istioctl x describe pod frontend-69577cb555-9th62
```

```
istioctl authn tls-check
```


{{% children showhidden="false" %}}
