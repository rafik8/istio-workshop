---
title: "Istio debugging tools"
chapter: true
weight: 1
---

# Debugging with Istio


1. check proxy status:

```
istioctl proxy-status
```

```
istioctl proxy-status
NAME                                                   CDS        LDS        EDS        RDS          PILOT                            VERSION
adservice-5968df5578-cvvst.hipster-app                 SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
cartservice-dd676648f-qh79z.hipster-app                SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
checkoutservice-568f8c99f8-d7sxx.hipster-app           SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
currencyservice-55fddd9499-qbcwb.hipster-app           SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
emailservice-764954c58d-4rvb2.hipster-app              SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
frontend-86568f6d79-6xqww.hipster-app                  SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
frontend-v0.1.3-564b57645-848j7.hipster-app            SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
istio-egressgateway-6f4569d68f-nt6wq.istio-system      SYNCED     SYNCED     SYNCED     NOT SENT     istio-pilot-586dc5646c-gfjsn     1.3.1
istio-ingressgateway-5d95d4cc88-9x77h.istio-system     SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
paymentservice-d76df5c58-7zzlc.hipster-app             SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
productcatalogservice-6bbbf99d6d-9fp8p.hipster-app     SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
recommendationservice-59b76d69d6-dlbvp.hipster-app     SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
redis-cart-58f6b79c49-4qtvw.hipster-app                SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1
shippingservice-6c6c84d8f8-ls4nt.hipster-app           SYNCED     SYNCED     SYNCED     SYNCED       istio-pilot-586dc5646c-gfjsn     1.3.1

```


1. Check list of service handled by the proxy for a pod:

```
istioctl proxy-config clusters POD-ID
```

```
istioctl proxy-config clusters frontend-86568f6d79-6xqww
```

```
SERVICE FQDN                                              PORT      SUBSET         DIRECTION     TYPE
BlackHoleCluster                                          -         -              -             STATIC
InboundPassthroughClusterIpv4                             -         -              -             ORIGINAL_DST
PassthroughCluster                                        -         -              -             ORIGINAL_DST
adservice.hipster-app.svc.cluster.local                   9555      -              outbound      EDS
calico-typha.kube-system.svc.cluster.local                5473      -              outbound      EDS
cartservice.hipster-app.svc.cluster.local                 7070      -              outbound      EDS
checkoutservice.hipster-app.svc.cluster.local             5050      -              outbound      EDS
currencyservice.hipster-app.svc.cluster.local             7000      -              outbound      EDS
default-http-backend.kube-system.svc.cluster.local        80        -              outbound      EDS
emailservice.hipster-app.svc.cluster.local                5000      -              outbound      EDS
frontend.hipster-app.svc.cluster.local                    80        -              outbound      EDS
frontend.hipster-app.svc.cluster.local                    80        http           inbound       STATIC
frontend.hipster-app.svc.cluster.local                    80        v1             outbound      EDS
frontend.hipster-app.svc.cluster.local                    80        v2             outbound      EDS
grafana.istio-system.svc.cluster.local                    3000      -              outbound      EDS
heapster.kube-system.svc.cluster.local                    80        -              outbound      EDS
istio-citadel.istio-system.svc.cluster.local              8060      -              outbound      EDS
istio-citadel.istio-system.svc.cluster.local              15014     -              outbound      EDS

```


Type of configuration: https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/operations/dynamic_configuration

1.Check list of listeners handled by the proxy for a pod:

```
istioctl proxy-config listeners POD-ID
```

```
istioctl proxy-config listeners frontend-86568f6d79-6xqww
ADDRESS         PORT      TYPE
10.16.2.12      8080      HTTP
10.16.2.12      15020     TCP
10.0.15.192     15443     TCP
10.0.15.30      15030     TCP
10.0.15.30      31400     TCP
10.0.4.109      15011     TCP
10.0.5.33       443       TCP
10.0.0.1        443       TCP
10.0.15.30      15031     TCP
10.0.2.81       6379      TCP
10.0.15.30      443       TCP
10.0.15.30      15029     TCP
10.0.15.30      15032     TCP
10.0.15.30      15443     TCP
10.0.0.10       53        TCP
10.0.15.192     443       TCP
10.0.13.84      443       TCP
10.0.15.30      15020     TCP
0.0.0.0         15010     TCP
0.0.0.0         5000      TCP
```


1. Open envoy proxy admin console:

```
kubectl port-forward POD-ID 15000
```


```
kubectl port-forward frontend-86568f6d79-6xqww 15000
```

Then hit: http://127.0.0.1:15000/


1. check a pod configuration:

```
istioctl x describe pod pod-id
```

```
istioctl x describe pod frontend-86568f6d79-6xqww
Pod: frontend-86568f6d79-6xqww
   Pod Ports: 8080 (server), 15090 (istio-proxy)
--------------------
Service: frontend
   Port: http 8080/HTTP
DestinationRule: frontend-destination-rule for "frontend"
   Matching subsets: v1
      (Non-matching subsets v2)
   No Traffic Policy
Pilot reports that pod is PERMISSIVE (enforces HTTP/mTLS) and clients speak HTTP
VirtualService: frontend-split
   Weight 50%
```

1. Check TLS status:
```
istioctl authn tls-check  pod-id
```
```
istioctl authn tls-check frontend-86568f6d79-6xqww
HOST:PORT                                                       STATUS     SERVER        CLIENT     AUTHN POLICY                                 DESTINATION RULE
adservice.hipster-app.svc.cluster.local:9555                    OK         HTTP/mTLS     HTTP       default/                                     -
calico-typha.kube-system.svc.cluster.local:5473                 OK         HTTP/mTLS     HTTP       default/                                     -
cartservice.hipster-app.svc.cluster.local:7070                  OK         HTTP/mTLS     HTTP       default/                                     -
checkoutservice.hipster-app.svc.cluster.local:5050              OK         HTTP/mTLS     HTTP       default/                                     -
currencyservice.hipster-app.svc.cluster.local:7000              OK         HTTP/mTLS     HTTP       default/                                     -
default-http-backend.kube-system.svc.cluster.local:80           OK         HTTP/mTLS     HTTP       default/                                     -
emailservice.hipster-app.svc.cluster.local:5000                 OK         HTTP/mTLS     HTTP       default/                                     -
frontend.hipster-app.svc.cluster.local:80                       OK         HTTP/mTLS     HTTP       default/                                     frontend-destination-rule/hipster-app
grafana.istio-system.svc.cluster.local:3000                     OK         HTTP          HTTP       grafana-ports-mtls-disabled/istio-system     -
heapster.kube-system.svc.cluster.local:80                       OK         HTTP/mTLS     HTTP       default/                                     -
istio-citadel.istio-system.svc.cluster.local:8060               OK         HTTP/mTLS     HTTP       default/                                     -
istio-citadel.istio-system.svc.cluster.local:15014              OK         HTTP/mTLS     HTTP       default/                                     -
istio-egressgateway.istio-system.svc.cluster.local:80           OK         HTTP/mTLS     HTTP       default/       
```


1. Get proxy logs:

```
kubectl logs po POD-ID -c istio-proxy
```

```
kubectl logs po frontend-86568f6d79-6xqww -c istio-proxy
```



{{% children showhidden="false" %}}
