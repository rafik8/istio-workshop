---
title: "Monitoring and Observability"
chapter: true
weight: 10
draft: true
---
# Monitoring and Observability

Bug:

Your need to have pod labeled with `version` to be supported by kiali.

![Kiali missing secret](/images/kiali-bug-version-label.png?width=50pc)


We need to update application manifest files under [microservices-demo/kubernetes-manifests](https://github.com/GoogleCloudPlatform/microservices-demo/tree/master/kubernetes-manifests) and then redploy with skaffold:

```
skaffold deploy
```

```
Starting deploy...
kubectl client version: 1.13
deployment.apps/adservice configured
service/adservice configured
deployment.apps/cartservice configured
service/cartservice configured
deployment.apps/checkoutservice configured
service/checkoutservice configured
deployment.apps/currencyservice configured
service/currencyservice configured
deployment.apps/emailservice configured
service/emailservice configured
deployment.apps/frontend configured
service/frontend configured
service/frontend-external configured
deployment.apps/loadgenerator configured
deployment.apps/paymentservice configured
service/paymentservice configured
deployment.apps/productcatalogservice configured
service/productcatalogservice configured
deployment.apps/recommendationservice configured
service/recommendationservice configured
deployment.apps/redis-cart configured
service/redis-cart configured
deployment.apps/shippingservice configured
service/shippingservice configured
```


Update Kiali cluster role:

```
kubectl replace -f ../kiali-clusterrole.yaml
```
