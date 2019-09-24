---
title: "Tracing"
chapter: true
weight: 2
---

1. Establish a secure tunnel to the Grafana pod using `kubectl port-forward`:

```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l \
    app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686
```
2. Establish the secure connection using the web preview on port 16686:

![Jaeger dashboard](/images/jaeger-dashboard.png?width=50pc)

We don't have any service running actually within the mesh that's why we have 0 service on tracing.
