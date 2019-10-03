---
title: "Egress Gateway"
chapter: true
weight: 8
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
gcloud redis instances create redis-cart --size=1 --region=$REGION_ID \
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

<!-- ```
kubectl run --generator=run-pod/v1 -i --tty busybox --image=busybox -- sh

istioctl x add-to-mesh busybox
``` -->

```
kubectl apply -f <(istioctl kube-inject -f $WORKSHOP_HOME/istio-workshop-labs/busybox.yaml)
```


```
kubectl exec -it busybox /bin/sh -n default
```

Execute telnet against Redis instance IP:

```
telnet REDIS_ADDR 6379
```
You should get the connection established:
```
# telnet 10.59.118.59  6379
Connected to 10.59.118.59
```

<!-- ```
kubectl -n default run -i --tty redisbox --image=gcr.io/google_containers/redis:v1 -- sh
```

```
kubectl exec -it redisbox-5b9cfb548f-jsln2 -c istio-proxy -- sh
``` -->


<!-- 5. Let's now change the cart service to use the managed Redis instance. Edit `cartservice-deployment.yaml` under `$WORKSHOP_HOME/istio-workshop-labs/cartservice-deployment.yaml`, then change the REDIS_ADDR value by the IP of the Redis HOST IP:

```
env:
- name: REDIS_ADDR
  value: "10.x.x.x:6379"
```


Then apply the configuration:


```
kubectl replace -f <(istioctl kube-inject -f $WORKSHOP_HOME/istio-workshop-labs/cartservice-deployment.yaml)
``` -->

6. to restrict traffic to external world, we will enable outbound traffic registration using by setting  `global.outboundTrafficPolicymode` to `REGISTRY_ONLY`:

```
helm upgrade --set global.outboundTrafficPolicymode=REGISTRY_ONLY  istio  $WORKSHOP_HOME/istio-$ISTIO_VERSION/install/kubernetes/helm/istio
```


7. Let's retest Redis connection:

```
kubectl exec -it busybox /bin/sh -n default

```

```
telnet REDIS_ADDR 6379
```

```
# telnet 10.59.118.59  6379
Connection closed by foreign host
```
As you mention, The managed Redis instance is not reachable:

8. We will configure a  `ServiceEntry` rule to enable service within the mesh to initiate connection to Redis instance outside of mesh network:

```
apiVersion: networking.istio.io/v1alpha3
kind: ServiceEntry
metadata:
  name: redis-serviceentry
spec:
  hosts:
  - redis.domain # not used
  addresses:
  - REDIS_ENDPOINT # VIPs
  ports:
  - number: 6379
    name: tcp-redis
    protocol: TCP
  location: MESH_EXTERNAL
```


Edit `$WORKSHOP_HOME/istio-workshop-labs/redis-serviceentry.yaml` then replace the `REDIS_ENDPOINT` with the IP address of you Redis managed instance the run the apply the configuration:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/redis-serviceentry.yaml
```

Then test the connection:

```
telnet 10.59.118.59  6379
Connected to 10.59.118.59
```

9. We will also create an ingress gateway and configure the service entry to flow the traffic via the egress gateway:

```
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: istio-egressgateway
spec:
  selector:
    istio: egressgateway
  servers:
  - port:
      number: 6379
      name: tcp-redis
      protocol: TCP
    hosts:
    - redis.domain # not used
    addresses:
    - REDIS_ENDPOINT # VIPs
---
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: redis-egressgateway
spec:
  host: istio-egressgateway.istio-system.svc.cluster.local
  subsets:
  - name: redis-serviceentry
```

Edit `$WORKSHOP_HOME/istio-workshop-labs/redis-egress.yaml` then replace the `REDIS_ENDPOINT` with the IP address of you Redis managed instance the run the apply the configuration:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/redis-egress.yaml
```

10. Test Redis after enabling the egress:

```
telnet 10.59.118.59  6379
Connected to 10.59.118.59
```
