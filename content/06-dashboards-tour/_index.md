---
title: "Dashboards Tour"
chapter: true
weight: 6
---
# Dashboards Tour

We will explore the different dashboards that we have enabled with Istio:


**Grafana**: exposes monitoring metrics for services running within the mesh.


**Tracing**: offers end to end tracing of http requests going through different layers and containers.

**Kiali**: Kiali offers visibility and real time observability for the services running within the mesh including Istio components.


{{% notice note %}}
Istio offers a flexible pluggable system so you could plug Istio to different external addons that you are already using such as GCP StackDriver, Amazon Cloud Watch, DataDog ... to list fews.
You could also integrated you own system using the **Mixer Configuration Model**: [Mixer overview](https://istio.io/docs/reference/config/policy-and-telemetry/mixer-overview/)

{{% /notice%}}

{{% children showhidden="false" %}}
