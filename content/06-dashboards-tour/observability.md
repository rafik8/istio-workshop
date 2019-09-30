---
title: "Observability"
chapter: true
weight: 3
---
<!-- 1. Establish a secure tunnel to the Grafana pod using `kubectl port-forward`:

```
kubectl -n istio-system port-forward   $(kubectl -n istio-system get pod -l \
    app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
``` -->

1. Use `istioctl dashboard` command line to Establish a secure tunnel to the Kiali pod:

<!-- Prior to 1.3:
using `kubectl port-forward`
```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod \
    -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
``` -->
```
istioctl dashboard kiali
```

1. The Kiali UI will be opened in your default web browser:

<!-- Establish the secure connection using the web preview on port 20001 -->

![Kiali missing secret](/images/kiali-missing-secret.png?width=50pc)


For the demo purpose, we will create a demo user account.

1. Execute the following command to create the Kiali secret:

<!-- ```
cat <<EOF | kubectl apply -f -
  apiVersion: v1
  kind: Secret
  metadata:
    name: kiali
    namespace: istio-system
    labels:
      app: kiali
  type: Opaque
  data:
    username: YWRtaW4=
    passphrase: YWRtaW4=
EOF
```
-->

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/kiali-secret.yaml
```

1. Refresh Kiali home page and the error message should disappear:

![Kiali login](/images/kiali-login.png?width=50pc)

1. Enter the login password and the Kiali page:

![Kiali dashbaord](/images/kiali-dashbaord.png?width=50pc)

1. The workload tab show the the service are running without sidecars as we haven't yet deployed the:

![Kiali missing secret](/images/kiali-missing-sidecar.png?width=50pc)

We will explore different Kiali features later with the application.
