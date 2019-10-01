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

- Service: it can be a service on the edge that communicate with the external world and need an ecrypted communcation.
- Namespace: services within the namespace communicate.
- Mesh:


## mTLS in practice

Let's ensure the scenario that we want that all  microservices forming the hispter app should communicate securely.

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


Note.: The first created rule should be called default.

1.  Enable mTLS in the `hipster-app` namespace:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/hipster-app-mtls.yaml
```

reaquest can not be received on the edge because of the mutual TLS mode STRICT.

add screen shot


Let's change it to PERMISSIVE: Pod is now accepting both secure and insure connections.


add screenshot.



Check TLS:

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


```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/hipster-app-mtls.yaml
policy.authentication.istio.io/default configured
Admins-MacBook-Pro:microservices-demo admin$ istioctl exp auth check frontend-dff555579-wcq58

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
