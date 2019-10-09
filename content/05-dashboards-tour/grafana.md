---
title: "Grafana"
chapter: true
weight: 1
---
# Grafana

1. Use `istioctl dashboard` command line to establish a secure tunnel to the Grafana pod:

```
istioctl dashboard grafana
```

1. In the Grafana Dashboard, click **Home → Istio**:

Istio comes with 6 default dashboards, you can add you owns or customise the existing ones:

![Istio default dashboard](/images/istio-default-dashboard.png?width=50pc)

1. Click **Home → Istio → Istio Performance Dashboard**:

![Dashboard change port](/images/grafana-istio-dashboards.png?width=50pc)

![Dashboard change port](/images/istio-performance-dashboard.png?width=50pc)


The dashboard shows Istio core components performance and resource utilization. We will use it later the others dashboard within the deployed application.


<!-- 1. In Cloud Shell, click **Web Preview icon → Change port**:

![Dashboard change port](/images/dashboard-change-port.png?width=25pc)

1. Enter port 3000, and click **Change and Preview**:

![Dashboard change port](/images/dashboard-change-port-1.png?width=25pc)

This will establish a connection from you local machine to your Cloud Shell machine on port 3000, which is connected to a Grafana pod inside of your Kubernetes cluster. -->
