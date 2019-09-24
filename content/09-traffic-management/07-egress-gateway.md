---
title: "Egress Gateway"
chapter: true
weight: 7
---

# Egress Gateway
In a typical enterprise scenario service have to declare their external(s) in a declarative way following the pattern of principle of least access. Service mesh solutions including Istio promote Egress gateway that control oubound connection and managed authorization in a declarative way.


## The pattern

The service mesh is deployed to no enable outbound traffic to external services. We need to explicitly allow traffic to external service using ServiceEntry.

A [ServiceEntry](https://istio.io/docs/reference/config/networking/v1alpha3/service-entry/) configuration enables services within the mesh to access a service not necessarily managed by Istio. The rule describes the endpoints, ports and protocols of a white-listed set of mesh-external domains and IP blocks that services in the mesh are allowed to access.

![Istio Egress diagram](/images/istio-egress-pattern.png)

## Egress in practice

The `Cart Service` is backed a Redis database used for caching items added to the cart.
In order to enhance the operations, Let's assumes that we decide to move to Redis database and use The managed Redis Service from _Google Cloud Platform_: [CLOUD MEMORYSTORE
](https://cloud.google.com/memorystore/).

![Istio Egress example](/images/istio-egress-example.png)


Let create Memory store instance using `gcloud` CLI:

1. Enable redis service:

```
gcloud services enable redis.googleapis.com
```

2. create a redis instance:

```
gcloud redis instances create redis-cart --size=3 --region=$REGION_ID \
    --zone=$ZONE_ID --redis-version=redis_4_0
```

```
Create request issued for: [redis-cart]
Waiting for operation [projects/srecon19-workshop-250603/locations/europe-west1/operations/operation-1569187161552-5932adb59b7bc-097e8af3-61183b2e] to complete...done.                                  
Created instance [redis-cart].
```
3. In the Google Cloud console, Select **Memory Store** to view the details of the create instance:


![GCP Redis instance details](/images/gcp-redis-instance-details.png)




4. Verify that the Redis service is reachable from the mesh network:

```
kubectl -n default run --generator=run-pod/v1 -i --tty busybox --image=busybox -- sh
```

```
telnet REDIS_ADDR 6379
```

telnet 10.0.0.3 6379

```
kubectl -n default run -i --tty redisbox --image=gcr.io/google_containers/redis:v1 -- sh
```

```
kubectl exec -it redisbox-5b9cfb548f-jsln2 -c istio-proxy -- sh
```

5. Edit `cartservice.yaml` under `$WORKSHOP_HOME/microservices-demo/kubernetes-manifests/cartservice.yaml`, then change the REDIS_ADDR value by the IP of the Redis HOST IP:

```
env:
- name: REDIS_ADDR
  value: "10.x.x.x:6379"
```


6.


7. Enable outbound traffic:

8. Test Redis

8. Configure ServiceEntry


9. Test Redis
