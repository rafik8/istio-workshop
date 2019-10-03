---
title: "Deploy application"
chapter: true
weight: 2
---

# Deploy Hipster Application


## Application namespace

1. Create a namespace for the application:
```
kubectl create namespace hipster-app
```

2. Set Kubernetes context to the created namespace:

```
kubectl config set-context --current --namespace=hipster-app
```


##  Deploy the application:

1. Deploy hipster application:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/hispter-app.yaml
```

1. Deploy the LoadBalancer:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/frontend-external-service.yaml
```

1. Deploy `loadgenerator` service:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/loadgenerator.yaml
```

{{% notice info %}}
The load generator application will simulate end user traffic.
{{% /notice%}}


1. Check that all pods are deployed:

```
kubectl get po,svc
```

```
NAME                                     READY   STATUS            RESTARTS   AGE
adservice-7f4dfb6d78-6p7g5               1/1     Running           0          64s
cartservice-58f94bf68d-rmlpx             1/1     Running           1          64s
checkoutservice-645d65f47-6r2s7          1/1     Running           0          63s
currencyservice-85649b744f-z7ztm         1/1     Running           0          63s
emailservice-797578d845-s6k9m            1/1     Running           0          62s
frontend-b7586cfd8-pm8pm                 1/1     Running           0          62s
loadgenerator-668dfddb69-fg4dl           0/1     PodInitializing   0          62s
paymentservice-7f97f76bb4-pxlzw          1/1     Running           0          61s
productcatalogservice-f9999d9c9-cfzn4    1/1     Running           0          61s
recommendationservice-7c7b7d9fbb-fdqtr   1/1     Running           0          61s
redis-cart-7d994ff866-4chxj              1/1     Running           0          60s
shippingservice-68c6d65498-8l6jv         1/1     Running           0          60s

NAME                            TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)        AGE
service/adservice               ClusterIP      10.19.246.130   <none>            9555/TCP       45h
service/cartservice             ClusterIP      10.19.248.197   <none>            7070/TCP       45h
service/checkoutservice         ClusterIP      10.19.253.129   <none>            5050/TCP       45h
service/currencyservice         ClusterIP      10.19.253.206   <none>            7000/TCP       45h
service/emailservice            ClusterIP      10.19.247.81    <none>            5000/TCP       45h
service/frontend                ClusterIP      10.19.245.8     <none>            80/TCP         45h
service/frontend-external       LoadBalancer   10.19.244.242   146.148.112.234   80:31937/TCP   45h
service/paymentservice          ClusterIP      10.19.248.228   <none>            50051/TCP      45h
service/productcatalogservice   ClusterIP      10.19.248.24    <none>            3550/TCP       45h
service/recommendationservice   ClusterIP      10.19.255.112   <none>            8080/TCP       45h
service/redis-cart              ClusterIP      10.19.241.10    <none>            6379/TCP       45h
service/shippingservice         ClusterIP      10.19.244.205   <none>            50051/TCP      45h

```
