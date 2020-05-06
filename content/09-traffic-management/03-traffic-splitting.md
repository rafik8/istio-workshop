---
title: "Traffic Splitting"
chapter: true
weight: 3
---
# Traffic Splitting:


## The pattern

![Istio traffic splitting](/images/istio-traffic-splitting.png?width=40pc)

## How it works

The traffic splitting is handled by two Istio Object:


| Object           | API                 | Version    |
| -----------------| --------------------|----------- |
| VirtualService   | networking.istio.io | v1alpha3   |
| DestinationRule  | networking.istio.io | v1alpha3   |

- **VirtualService**: A [VirtualService](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) defines a set of traffic routing rules to apply when a host is addressed.

- **DestinationRule**: A [DestinationRule](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/) defines policies that apply to traffic intended for a service after routing has occurred.

We create a `VirtualService` that list the different variation with their the weight:

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: service-a
spec:
  hosts:
    - service-a
  http:
    - route:
        - destination:
            host: service-a
            subset: v1
          weight: 80
        - destination:
            host: service-a
            subset: v2
          weight: 20
```



Then the `DestinationRule` is responsible for defining the destination of the traffic and the traffic policy:

```
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: property-business-service
spec:
  host: property-business-service
  subsets:
    - name: v1
      labels:
        version: "1.0"
    - name: v2
      labels:
        version: "1.1"
```


## Traffic Splitting in practice


<!-- First we need to create to create a new version of the frontend service:

1. create a copy of `frontend.yaml` under `$WORKSHOP_DIR/microservices-demo/kubernetes-manifests/` and name it `frontend-0.1.3.yaml`.

2. Edit `frontend-0.1.3.yaml` and change the label `version` to `0.1.3`.

3. Edit `$WORKSHOP_DIR/microservices-demo/src/frontend/templates/product.html`, on line 39 change `btn-primary` to `btn-success`:

```
...
<button type="submit" class="btn-success btn-info btn-lg ml-3">Add to Cart</button>
...
```

Change docker images with the full path of GCP:

4. Tag the version:



4. Build the images:

```
skaffold run -p gcb --default-repo=gcr.io/$PROJECT_ID
```
it will take around 3 minutes.
Meanwhile let's explore manifests: -->

Let's assume the we have new version of the `frontend` service where the advertise banner is in the top:

![Frontend service versions](/images/frontend-versions.png?width=50pc)

1. The Deploy  the new version:

```
kubectl apply -f <(istioctl kube-inject -f $WORKSHOP_HOME/istio-workshop-labs/frontend-0.1.3.yaml)
```

1. Extend the `VirtualService` of frontend service the destination:

```
http:
- match:
  - uri:
      prefix: /
  route:
  - destination:
      host: frontend
      port:
        number: 80
      subset: v1
    weight: 80
  - destination:
      host: frontend
      port:
        number: 80
      subset: v2
    weight: 50
```


Apply the VirtualService:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/frontend-virtualservice.yaml
```

1. Create a `DestinationRule` to specify the traffic destination:

```
apiVersion: networking.istio.io/v1alpha3
kind: DestinationRule
metadata:
  name: frontend-destination-rule
spec:
  host: frontend
  subsets:
    - name: v1
      labels:
        version: "v0.1.2"
    - name: v2
      labels:
        version: "v0.1.3"
```

Apply the DestinationRule:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/frontend-destinationrule.yaml
```

1. Wait a few second so the configuration will be pushed to envoy sidecars the refresh you browser:
You have to get `version 1` **four time** before getting `version 2` of the frontend service rendered.



## Check traffic on Kiali:

As mentioned earlier, Kiali provides a great real time view of what is going on the cluster:

<!-- run the script as following:

```
./$WORKSHOP_HOME/istio-workshop-labs/hipster-curl-loop.sh
``` -->

1. Launch Kiali dashboard:
```
istioctl dashboard kiali
```
1. Select **hipster-app** namespace **Graph** on the left menu then select **Versioned App Graph**, **Request Percentage** as Options:

![Kiali Traffic Splitting](/images/kiali-traffic-splitting-1.png?width=30pc)

![Kiali Traffic Splitting](/images/kiali-traffic-splitting-2.png?width=30pc)

1. On the **Display menu**, check  **Display menu**:

![Kiali Traffic Splitting](/images/kiali-traffic-splitting-3.png?width=10pc)

Graph will show the real traffic percentage between the different versions:

![Kiali Traffic Splitting](/images/kiali-traffic-splitting-5.png?width=50pc)



<!-- ## Exercise:
Traffic Shifting. -->
