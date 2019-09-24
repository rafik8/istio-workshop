---
title: "Grafana"
chapter: true
weight: 1
---

1. Establish a secure tunnel to the Grafana pod using `kubectl port-forward`:

Prior to 1.3:

```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod \
    -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
```

1.3:
```
istioctl dashboard grafana
```

2. In Cloud Shell, click **Web Preview icon → Change port**:

![Dashboard change port](/images/dashboard-change-port.png?width=25pc)

3. Enter port 3000, and click **Change and Preview**:

![Dashboard change port](/images/dashboard-change-port-1.png?width=25pc)

This will establish a connection from you local machine to your Cloud Shell machine on port 3000, which is connected to a Grafana pod inside of your Kubernetes cluster.


4. In the Grafana Dashboard, click **Home → Istio**:

![Istio default dashboard](/images/istio-default-dashboard.png?width=50pc)

Istio comes with 6 dashboards, you can add you owns or customize the existing ones:


4. Click **Home → Istio Performance Dashboard**:

![Dashboard change port](/images/grafana-istio-dashboards.png?width=50pc)



![Dashboard change port](/images/istio-performance-dashboard.png?width=50pc)


The dashboard show Istio core components performance and resource utilisation. We will you use later the others dashboard within the deployed application.
