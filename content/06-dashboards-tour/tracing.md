---
title: "Tracing"
chapter: true
weight: 2
---

1. Use `istioctl dashboard` command line to Establish a secure tunnel to the Jaeger pod:

<!-- ```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l \
    app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686
``` -->

```
istioctl dashboard jaeger
```

2. The Jaeger UI will be opened in your default web browser:

![Jaeger dashboard](/images/jaeger-dashboard.png?width=50pc)

We don't have any service running actually within the mesh that's why we have zero service with tracing.
