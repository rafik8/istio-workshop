---
title: "Istio Architecture"
chapter: true
weight: 3
---

# Istio Architecture and Building Blocks

{{% notice info  %}}
**Istio goals**: develop an open technology that provides a uniform way to connect, secure, manage and monitor a network of microservices regardless of the platform source or vendor.
{{% /notice%}}

## Architecture

Istio service mesh provides a modular architecture similar to kubernetes logically splitted into a **control plane** and a **data plane**:

**The control plane**: is the brain of the main network who manage, control, and supervise the network of microservies.

The control plane manages and configures the proxies to route traffic. Additionally, the control plane configures Mixers to enforce policies and collect telemetry.

**The data plane**: The data plane is composed of a set of intelligent proxies (Envoy) deployed as sidecars.

These proxies mediate and control all network communication between microservices along with Mixer, a general-purpose policy and telemetry hub.

The sidecars deployed within the services and acting as proxy form the service mesh network.

The following diagram illustrates the basic architecture:

![Service Mesh Architecture](/images/servicemesh-highlevel-architecture.png?width=40pc  "Service Mesh Architecture")


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


![Istio Building Blocks](/images/istio-components-interaction.png  "Istio Building Blocks")

- The ingress controller is responsible for allowing and redirecting the inbound traffic to the services running inside the service mesh.
- The egress controller is responsible for allowing outbound traffic from the service mesh. If an application should connect, for example, to an external database or service, such configuration should be explicitly defined for the egress controller.
- Pilot and Galley are responsible for the mesh configuration: they pull data from Kubernetes API Server and mix it with the local configuration defined within the mesh then push the configuration to different proxies forming the mesh.

{{% notice info %}}
  As a long term goal, Galley will the only be responsible for configuration ingestion from Kubernetes API and Pilot for configuration within the mesh.
{{% /notice%}}

- Citadel push tls certificate to services enabling mutual TLS.

- Mixer has two roles: gather metrics from the different components and enforce policy by double check each request. In a high level deployment scenario Telemetry and Policy check should be deployed separately.

- Dashboards gather metrics from the telemetry service and display it in a user friendly format.


{{% notice tip %}}
If you want to check the list of list of  mandatory components, check [istio minimal profile](https://github.com/istio/istio/blob/master/install/kubernetes/helm/istio/values-istio-minimal.yaml).
{{% /notice%}}


## Upstream vs Downstream

**Upstream** connections are the service Envoy is initiating the connection to.

**Downstream** connections are the client that is initiating a request through Envoy.

![Envoy downstream upstream diagram](/images/istio-upstream-downstream.png?width=50pc)

## Reserved ports

The following ports and protocols are used by Istio. Ensure that there are no TCP headless services using a TCP port used by one of Istio's services.

| Port   | Protocol | Used by | Description |
|--------|----------|---------|-------------|
| 8060   | HTTP     | Citadel | GRPC server |
| 9090   | HTTP     | Prometheus | Prometheus |
| 9091   | HTTP     | Mixer   | Policy/Telemetry |
| 9093   | HTTP     | Citadel |                  |
| 15000  | TCP      | Envoy   | Envoy admin port (commands/diagnostics) |
| 15001  | TCP      | Envoy   | Envoy |
| 15004  | HTTP     | Mixer, Pilot | Policy/Telemetry - `mTLS` |
| 15010  | HTTP     | Pilot   | Pilot service - XDS pilot - discovery |
| 15011  | TCP      | Pilot   | Pilot service - `mTLS` - Proxy - discovery |
| 15014  | HTTP     | Citadel, Mixer, Pilot | Control plane monitoring |
| 15030  | TCP      | Prometheus | Prometheus |
| 15090  | HTTP     | Mixer   | Proxy |
| 42422  | TCP      | Mixer   | Telemetry - Prometheus |
