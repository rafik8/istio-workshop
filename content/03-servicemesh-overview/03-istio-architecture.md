---
title: "Istio Architecture and Building Blocks"
chapter: true
weight: 3
---

# Istio Architecture and Building Blocks

## Architecture

Istio service mesh provides a modular architecture similar to kubernetes logically splitted into a **control plane** and a **data plane**:

**The control plane**: is the brain of the main network who manage, control, and supervise the network of microservies.

**The data plane**: deployed within the services forming the service mesh as sidecars and acting as proxy.


![Service Mesh Architecture](/images/servicemesh-highlevel-architecture.png  "Service Mesh Architecture")


## Building blocks



|  Group            | Component  |   Description                                                           | Mandatory| Comment     |
| ------------------|:----------:| :----------------------------------------------------------------------:| :--------:|:----------:|
| **Control plane** | Pilot      | Responsible for service discovery and configuring envoy sidecar proxies |  **√**    |            |
| **Control plane** | Galley     | Configuration ingestion for istio components                            |           |            |
| **Control plane** | Sidecar injector | Inside envoy sidecar for enabled namespaces                       |           |            |
| **Control plane** | Citadel    | Automated key and certificate management                                |           |            |
| **Control plane** | Policy     | Policy enforcement                                                      |           |            |
| **Control plane** | Telemetry  | Gather telemetry data                                                   |           |            |
| **Control plane** | Ingresss Gateway | Manage inbound connection to the service mesh                     |           |            |
| **Control plane** | Egress Gateway | Manage outbound connection from the service mesh                    |           |            |
| **Control plane** | Istio CNI  | Network initialisation                                                  |           |            |
| **Control plane** | Prometheus | Metrics collections                                                     |           |            |
| **Control plane** | Core DNS   | DNS resolution in a multicluster gateways deployment                    |           |            |
| **Dashboard**     | Grafana    | Monitoring dashboard                                                    |           |            |
| **Dashboard**     | Jaeger     | Distributed tracing                                                     |           |            |
| **Dashboard**     | Kiali      | Observability dashboard                                                 |           |            |
| **Dashboard**     | Service graph  | Service dependency                                                  |           | Deprecated in favor of Kiali|
| **Data plane**    | Envoy proxy|  Proxy injected as a sidecar                                            |           |            |

{{% notice tip %}}
If you want to check the list of list of  mandatory components, check [istio minimal profile](https://github.com/istio/istio/blob/master/install/kubernetes/helm/istio/values-istio-minimal.yaml).
{{% /notice%}}
