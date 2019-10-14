---
title: "Observability"
chapter: true
weight: 1
---
# Visualizing Your Mesh with Kiali

Kiali enable service mesh visualizing and Observability by providing a set of views to inspect services running within the mesh:

1. Execute the following command to open the Kiali UI:

```
istioctl dashboard kiali
```

1. **Overview** view: The Overview page displays a summary of all the namespaces with the numbers of applications, health Check status and the traffic.


![Kiali Service Overview](/images/kiali-service-overview.png)

2.**Graph** view: Observe Real time traffic with Kiali graph:

![Kiali Service Graph](/images/kiali-service-graph.png)

3. To view a summary of metrics for a service, select the service in the graph and a panel on the right will appear to display its metric details.

![Kiali Service Metrics](/images/kiali-service-metrics.png)
