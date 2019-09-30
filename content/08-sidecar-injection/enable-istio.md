---
title: "Enable Istio for Hispter app"
chapter: true
weight: 1
---

# Enable Istio for Hispter app

1. We need first to label the namespace that will host the application with `istio-injection=enabled`. this is will enable automatic deploy of the envoy sidecar for each pod deployed.

```
kubectl label namespace hipster-app istio-injection=enabled
```

2. We need to redeploy the hipster application. Let's delete the application and redeploy it so pods will be created with the sidecar.

```
skaffold delete
```

Deploy the application:

```
skaffold run --default-repo=gcr.io/$PROJECT_ID
```

```
$WORKSHOP_HOME/istio-workshop-labs/deploy-hipster-app.sh
```

```
kubectl apply -f <(istioctl kube-inject -f $WORKSHOP_HOME/istio-workshop-labs/hispter-app.yaml)
```


```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/loadgenerator.yaml
```



3. Check that all services and pods are correctly deployed and running:

```
kubectl get services
```

```
NAME                    TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)        AGE
adservice               ClusterIP      10.19.240.108   <none>         9555/TCP       3m21s
cartservice             ClusterIP      10.19.246.92    <none>         7070/TCP       3m21s
checkoutservice         ClusterIP      10.19.243.138   <none>         5050/TCP       3m20s
currencyservice         ClusterIP      10.19.241.45    <none>         7000/TCP       3m20s
emailservice            ClusterIP      10.19.245.193   <none>         5000/TCP       3m19s
frontend                ClusterIP      10.19.240.49    <none>         80/TCP         3m19s
frontend-external       LoadBalancer   10.19.250.219   34.77.233.29   80:31330/TCP   3m19s
paymentservice          ClusterIP      10.19.245.87    <none>         50051/TCP      3m18s
productcatalogservice   ClusterIP      10.19.242.188   <none>         3550/TCP       3m18s
recommendationservice   ClusterIP      10.19.255.178   <none>         8080/TCP       3m18s
redis-cart              ClusterIP      10.19.246.29    <none>         6379/TCP       3m17s
shippingservice         ClusterIP      10.19.254.34    <none>         50051/TCP      3m17s
```

```
kubectl get pods
```

```
NAME                                     READY   STATUS    RESTARTS   AGE
adservice-76b7788686-k5bhf               2/2     Running   0          2m49s
cartservice-8458b9fb4-dlm4h              2/2     Running   2          2m49s
checkoutservice-5f85f9499f-4qxrk         2/2     Running   0          2m48s
currencyservice-74894b6c7-q4j24          2/2     Running   0          2m48s
emailservice-5bfd4f4957-7vspz            2/2     Running   0          2m48s
frontend-5cb67f6b68-5fvw4                2/2     Running   0          2m47s
loadgenerator-688d4449d9-q6v5k           2/2     Running   0          2m47s
paymentservice-5bb99c59b8-mm5w5          2/2     Running   0          2m46s
productcatalogservice-f4c688fbc-s4zcj    2/2     Running   0          2m46s
recommendationservice-6b7c96f6f8-7qpd6   2/2     Running   0          2m46s
redis-cart-6f97c46c78-5xj27              2/2     Running   0          2m45s
shippingservice-7c569b6b8b-hbfgs         2/2     Running   0          2m45s
```

```
kubectl get service frontend-external
```


We need also to update the application DNS entry with the new DNS entry:

![Cloud DNS update service record](/images/gcp-cloud-dns-record-updated.png?width=40pc)



By enabling Istio for the application, I was able to observe the microservices at the app layer with Grafana, Jaeger, or Kiali, without needing to modify the service code or rebuild the container images.
