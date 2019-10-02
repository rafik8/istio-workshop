---
title: "Ingress Gateway Exercise Solution"
chapter: true
weight: 1
---

## Ingress Gateway Exercise Solution

1. Get the IP address of the Ingress Gateway:

```
echo $INGRESS_IP
```

1. Configure Cloud DNS with `dashboard.**dashboard**-srecon19.innovlabs.io` to point to the Ingress Gateway:


![Cloud DNS dashbaord record](/images/gcloud-dns-record-dashboard.png?width=40pc)


1. Create a Gateway for Istio addons:

```
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: addons-gateway
spec:
  selector:
    istio: ingressgateway # use istio default controller
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "dashboard.trainee001-srecon19.innovlabs.io"
```

1. Create Virtual Service for both Kiali and Jager:

**Kiali**:

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: kiali
spec:
  hosts:
  - "dashboard.trainee001-srecon19.innovlabs.io"
  gateways:
  - addons-gateway
  http:
  - match:
    - uri:
        prefix: /kiali
    route:
    - destination:
        host: kiali
        port:
          number: 20001
```

**Jaeger**:

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: tracing
spec:
  hosts:
  - "dashboard.trainee001-srecon19.innovlabs.io"
  gateways:
  - addons-gateway
  http:
  - match:
    - uri:
        prefix: /jaeger
    route:
    - destination:
        host: tracing
        port:
          number: 80
```



1. Apply the configuration:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/addons-ingress.yaml -n istio-system

```
