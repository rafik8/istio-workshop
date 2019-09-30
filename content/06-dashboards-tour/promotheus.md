---
title: "Prometheus"
chapter: true
weight: 4
---
1. Use `istioctl dashboard` command line to Establish a secure tunnel to the Prometheus pod::

<!-- ```
kubectl -n istio-system port-forward   $(kubectl -n istio-system get pod -l \
    app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090
``` -->
```
istioctl dashboard jaeger
```

2. The Prometheus UI will be opened in your default web browser:

![Jaeger dashboard](/images/promotheus-dashboard.png?width=50pc)

3. Choose the api server request count then click **execute**:

![Promethus request count](/images/prometheus-request-count.png?width=50pc)

The number of request to the api server is displayed for each requester.
