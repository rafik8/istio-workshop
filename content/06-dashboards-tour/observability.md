---
title: "Observability"
chapter: true
weight: 3
---
1. Establish a secure tunnel to the Grafana pod using `kubectl port-forward`:

```
kubectl -n istio-system port-forward   $(kubectl -n istio-system get pod -l \
    app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
```

2. Establish the secure connection using the web preview on port 20001:

![Kiali missing secret](/images/kiali-missing-secret.png?width=50pc)



we need to create a secret for Kiali.

4. Execute the following command to create the Kiali secret:

```
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

5. Refresh Kiali home page and the error message should disappear:

![Kiali login](/images/kiali-login.png?width=50pc)

6. Enter the login password and the Kiali page:

![Kiali dashbaord](/images/kiali-dashbaord.png?width=50pc)

We will explore different Kiali features later with the application.
