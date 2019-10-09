---
title: "Observability"
chapter: true
weight: 3
---
# Observability

1. Use `istioctl dashboard` command line to establish a secure tunnel to the Kiali pod:

  ```
  istioctl dashboard kiali
  ```

1. The Kiali UI will be opened in your default web browser: ![Kiali missing secret](/images/kiali-missing-secret.png?width=50pc)


1. For the demo purpose, we will create a demo user account. Execute the following command to create the Kiali secret:

  ```
  kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/kiali-secret.yaml
  ```

1. Refresh Kiali home page and the error message should disappear: ![Kiali login](/images/kiali-login.png?width=50pc)


1. Enter the login password and the Kiali page: ![Kiali dashbaord](/images/kiali-dashbaord.png?width=50pc)


1. The workload tab show the the service are running without sidecars as we haven't yet deployed the:

![Kiali missing secret](/images/kiali-missing-sidecar.png?width=50pc)


We will explore different Kiali features later with the application.
