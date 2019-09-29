---
title: "Circuit Breaker"
chapter: true
weight: 6
---
# Circuit Breaker

## The pattern

Circuit breaking enables to fail quickly and apply back pressure downstream as soon as possible. Istio enforces circuit breaking limits at the network level using envoy sidecar as opposed to having to configure and code each application independently.

There is two types of Circuit Breaker:

**Maximum Connections**: Maximum number of connections to a service. Any excess connection will be pending in a queue. You can modify this number by changing the `maxConnections` field.
**Maximum Pending Requests**: Maximum number of pending requests to a service. Any excess pending requests will be denied. You can modify this number by changing the `http1MaxPendingRequests` field.


## How it works

The Fault injection is handled by the following Istio Object:


| Object           | API                 | Version    |
| -----------------| --------------------|----------- |
| DestinationRule  | networking.istio.io | v1alpha3   |


![Istion Circuit Breaker](/images/istio-circuit-breaker.png?width=70pc)

## Circuit Breaker in practice

__Scenario__: We set redis-cart maximum connections to 1 and Maximum pending requests to 1. Thus, if we sent more than 2 requests at once to redis-cart, redis-cart will have 1 pending request and deny any additional requests until the pending request is processed.

```
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: redis-cb
  namespace: hipster-app
spec:
  host: redis-cart
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 1
      http:
        http1MaxPendingRequests: 1
        maxRequestsPerConnection: 1
    outlierDetection:
      consecutiveErrors: 1
      interval: 1s
      baseEjectionTime: 3m
      maxEjectionPercent: 100
```

Istio will detect any host that triggers a server error (5XX code) in the redis-cart's Envoy and eject the pod out of the load balancing pool for 3 minutes.

Applying a back pressure avoid a full system outage and preserve a minimal service. Besides, we could apply other Kubernetes patterns to scale the service (Horizontal Pod Autoscaler, ...).


1. Apply the circuit breaker rule:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/redis-circuitbreaker.yaml
```

2. on Kiali dashboard, check the `redis-cart` service:

![Redis Circuit Breaker](/images/redis-circuit-breaker.png?width=70pc)
