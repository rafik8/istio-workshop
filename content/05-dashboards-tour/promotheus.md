---
title: "Prometheus"
chapter: true
weight: 4
---
# Prometheus

1. Use `istioctl dashboard` command line to Establish a secure tunnel to the Prometheus pod::

```
istioctl dashboard prometheus
```

1. The Prometheus UI will be opened in your default web browser:
![Jaeger dashboard](/images/promotheus-dashboard.png?width=50pc)

1. Choose the api server request count then click **execute**:
![Promethus request count](/images/prometheus-request-count.png?width=50pc)
The number of request to the api server is displayed for each requester.
