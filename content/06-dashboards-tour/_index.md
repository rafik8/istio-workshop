---
title: "Dashboards Tour"
chapter: true
weight: 6
---
# Dashboards Tour

We will explore the different dashboards that come with Istio:


**Grafana**: exposes monitoring metrics for services running within the mesh.


**Tracing**: offers end to end tracing of http requests which enable easy debugging when a http request go through different layers and containers.

**Kiali**: Kiali offers a real time observability for the services running within the mesh including Istio components.


{{% notice note %}}
Istio offers a flexible pluggable system so you could plug Istio to different external addons that you are alredy using such as GCP StackDriver, Amazon Cloud Watch, DataDog ... to list fews.
You could also integrated you own system using the **Mixer Configuration Model**: [Mixer overview](https://istio.io/docs/reference/config/policy-and-telemetry/mixer-overview/)

{{% /notice%}}

{{% children showhidden="false" %}}
