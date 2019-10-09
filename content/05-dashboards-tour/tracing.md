---
title: "Tracing"
chapter: true
weight: 2
---

# tracing

1. Use `istioctl dashboard` command line to establish a secure tunnel to the Jaeger pod:


```
istioctl dashboard jaeger
```

2. The Jaeger UI will be opened in your default web browser:

![Jaeger dashboard](/images/jaeger-dashboard.png?width=50pc)

We don't have any service running actually within the mesh that's why we have zero service with tracing.
