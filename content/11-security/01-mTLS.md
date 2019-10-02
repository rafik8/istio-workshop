---
title: "Traffic encryption using mTLS"
chapter: true
weight: 1
---
# Traffic encryption using mTLS

## Introduction
Transport authentication, also known as service-to-service authentication ensures that traffic is encrypted on transit between services.

## How it works

![Istio security architecture](/images/istio-security-architecture.svg)


Mutual TLS can be enabled on 3 levels:

- **Service**: Enable mTLS for a subset of services. It can be a service on the edge that communicate with the external world and need an encrypted communication.
- **Namespace**: Enable mTLS for a specific namespace. Services within the namespace will have mTLS installed and communicate using TLS.
- **Mesh**: Enable mTLS for the entire mesh network.


## mTLS in practice

Let's ensure the scenario that we want that all microservices forming the hipster app should communicate securely.

The mTLS is is handled by the following Istio Resources:

| Resource        | API                     | Version    |
| ----------------| ------------------------|----------- |
| Policy          | authentication.istio.io | v1alpha1   |
| DestinationRule | networking.istio.io     | v1alpha3   |

**[Policy](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/)**:

**[DestinationRule](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/)**: defines policies that apply to traffic intended for a service after routing has occurred.



TLS connection mode:

| Name            | Description                                             |
| ----------------| --------------------------------------------------------|
| **DISABLE**         | Do not setup a TLS connection to the upstream endpoint. |
| **SIMPLE**          | Originate a TLS connection to the upstream endpoint.    |
| **MUTUAL**          | Secure connections to the upstream using mutual TLS by presenting client certificates for authentication.   |
| **ISTIO_MUTUAL**    | Secure connections to the upstream using mutual TLS by presenting client certificates for authentication. Compared to Mutual mode, this mode uses certificates generated automatically by Istio for mTLS authentication.     |





We need to define a `Policy` and a `DestinationRule` as following:

**Policy**:

```
apiVersion: "authentication.istio.io/v1alpha1"
kind: "Policy"
metadata:
  name: "default"
  namespace: "hipster-app"
spec:
  peers:
  - mtls:
      mode: STRICT
```

**DestinationRule**:

```
apiVersion: "networking.istio.io/v1alpha3"
kind: "DestinationRule"
metadata:
  name: "default"
  namespace: "hipster-app"
spec:
  host: "*.hipster-app.svc.cluster.local"
  trafficPolicy:
    tls:
      mode: ISTIO_MUTUAL
```

Note.: The first created rule should be called default.

1.  Enable mTLS for the `hipster-app` namespace:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/hipster-app-mtls.yaml
```

2. Apply a `DestinationRuleq
```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/hipster-app-destinationrule.yaml
```

Requests can not be received on the edge because of the mutual TLS STRICT mode:  service communication should be only using TLS:

add screenshot


Let's change it to `PERMISSIVE` and apply the configuration:

![Hipster app mtls permissive](/images/hipster-app-permissive-mtls.png?width=50pc)

Pods are now accepting both secure and insure connections. The connection is secured between frontend and the backend services but using http between `ingressgateway` and `frontend` (PERMISSIVE mode when TLS is not possible).







In order to be able to access the application in the a secure mode, we need to configure the ingress gateway to handle to TLS certificate.




## Check TLS:

**Apply STRICT mode:**

```
istioctl exp auth check frontend-dff555579-wcq58
```


```shell
Checked 2/45 listeners with node IP 10.16.2.11.
LISTENER             CERTIFICATE                   mTLS (MODE)      JWT (ISSUERS)     RBAC (RULES)
10.16.2.11_8080      /etc/certs/cert-chain.pem     yes (STRICT)     no (none)         no (none)
10.16.2.11_15020     none                          no (none)        no (none)         no (none)

Checked 54/61 outbound clusters.
HOST:PORT                                     CERTIFICATE     VALIDATE PEER CERTIFICATE
kubernetes.default:443                        none            none
adservice.hipster-app:9555                    none            none
cartservice.hipster-app:7070                  none            none
checkoutservice.hipster-app:5050              none            none
currencyservice.hipster-app:7000              none            none
emailservice.hipster-app:5000                 none            none
frontend.hipster-app:80                       none            none
frontend-external.hipster-app:80              none            none
paymentservice.hipster-app:50051              none            none
productcatalogservice.hipster-app:3550        none            none
recommendationservice.hipster-app:8080        none            none
redis-cart.hipster-app:6379                   none            none
```


**Apply PERMISSIVE mode:**

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/hipster-app-mtls.yaml
policy.authentication.istio.io/default configured
```

```
istioctl exp auth check frontend-dff555579-wcq58

Checked 2/45 listeners with node IP 10.16.2.11.
LISTENER               CERTIFICATE                   mTLS (MODE)          JWT (ISSUERS)     RBAC (RULES)
10.16.2.11_8080[0]     /etc/certs/cert-chain.pem     yes (PERMISSIVE)     no (none)         no (none)
10.16.2.11_8080[1]     none                          no (PERMISSIVE)      no (none)         no (none)
10.16.2.11_15020       none                          no (none)            no (none)         no (none)

Checked 54/61 outbound clusters.
HOST:PORT                                     CERTIFICATE     VALIDATE PEER CERTIFICATE
kubernetes.default:443                        none            none
adservice.hipster-app:9555                    none            none
cartservice.hipster-app:7070                  none            none

```
