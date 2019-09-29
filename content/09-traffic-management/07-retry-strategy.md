---
title: "Retry Strategy"
chapter: true
weight: 7
---
# Retry Strategy

## The pattern
A retry setting specifies the maximum number of times an Envoy proxy attempts to connect to a service if the initial call fails. Retries can enhance service availability and application performance by making sure that calls don’t fail permanently because of transient problems such as a temporarily overloaded service or network.

A retry is configured using two fields:

- `attempts`: The number of retries before activating a circuit breaker.

- `perTryTimeout`: the timeout per each request.


## How it works

The Fault injection is handled by the following Istio Object:


| Object           | API                 | Version    |
| -----------------| --------------------|----------- |
| VirtualService   | networking.istio.io | v1alpha3   |


![Istio retry](/images/istio-retry-strategy.png?width=70pc)


## Retry in practice


1. We will configure shippingservice to have three times retry in case of failures, this increasing the chance of resulting in a successful response in case of temporary outage. we configure also the retry time to 3 seconds.

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: shippingservice-retry
spec:
  hosts:
  - shippingservice
  http:
  - route:
    - destination:
        host: shippingservice
    retries:
      attempts: 3
      perTryTimeout: 3s
```

2. Apply the  configuration with the command below:


```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/shippingservice-retry.yaml
```

3.  Check out the Grafana graphs to view the success rate:

![Grafana Circuit Breaker](/images/grafana-circuit-breaker.png?width=70pc)
