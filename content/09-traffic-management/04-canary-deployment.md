---
title: "Canary Deployment"
chapter: true
weight: 4
---

# Canary release with Istio

In Canary Deployments, newer versions of services are incrementally rolled out to users to minimize the risk and impact of any bugs introduced by the newer version. To begin incrementally routing traffic to the newer version of the frontend service, modify the original **VirtualService** rule:


![Canary Deployment](/images/canary-deployment.png?width=30pc)

1. Modify the `weight` in `$WORKSHOP_HOME/istio-workshop-labs/frontend-virtualservice.yaml` in order to shift traffic gradually:

![Canary Deployment sample](/images/canary-deployment-code.png?width=30pc)
