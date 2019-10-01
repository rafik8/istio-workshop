---
title: "Distributed Tracing"
chapter: true
weight: 3
---

# Distributed Tracing with Jaeger


1. Execute the following command to open the Kiali UI:

```
istioctl dashboard jaeger
```

2. Jaeger DAG graph overview:

Directed Acyclic Graph(DAG)  is a graph that is directed and without cycles connecting the other edges. The graph enables to detect any cyclic dependencies between the microservices.

![Jaeger DAG Graph](/images/jaeger-dag-graph.png)
