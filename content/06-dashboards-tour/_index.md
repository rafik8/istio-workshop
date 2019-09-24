---
title: "Dashboards Tour"
chapter: true
weight: 6
---

# Dashboards Tour

We will explore the different dashboards that come with Istio:


**Grafana**: The monitoring dashboard gathering.


**Tracing**: offer tracing of http request end to end enabling easy debugging specially when a http request go through different layer and containers.



**Kiali**: Kiali offer a real time obervability for the service running within the mesh including istio components


{{% notice note %}}
Istio offers a flexible pluggable system so you could plug istio to different extenal services that you are used such as GCP StackDriver, Amazon Cloud Watch, DataDog ... to list fews.
You could also integrated you own system using the **Mixer Configuration Model**: [Mixer overview](https://istio.io/docs/reference/config/policy-and-telemetry/mixer-overview/)

See the documentation:

{{% /notice%}}

{{% children showhidden="false" %}}
