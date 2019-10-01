---
title: "Traffic Splitting"
chapter: true
weight: 3
---
# Traffic Splitting:


## The pattern

TODO: Add Schema here for illustration.

## How it works

The traffic splitting is handled by two Istio Object:


| Object           | API                 | Version    |
| -----------------| --------------------|----------- |
| VirtualService   | networking.istio.io | v1alpha3   |
| DestinationRule  | networking.istio.io | v1alpha3   |

- **VirtualService**: A [VirtualService](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) define a set of traffic routing rules to apply when a host is addressed.

- **DestinationRule**: A [DestinationRule](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/) define policies that apply to traffic intended for a service after routing has occurred.


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


5. Deploy  the manifests:

```
kubectl apply -f <(istioctl kube-inject -f $WORKSHOP_HOME/istio-workshop-labs/frontend-0.1.3.yaml)
```

6. Apply DestinationRule:


```
kubectl apply -f ../frontend-ingress.yaml
```

```
kubectl apply -f ../frontend-destinationrule.yaml
```
7. Refresh you browser:



8. Check traffic on Kiali:

As mentionned earlier, Kiali provides a great real time view of what is going on the cluster.

run the script as following:

```
./$WORKSHOP_HOME/istio-workshop-labs/hipster-curl-loop.sh
```


Launch Kiali dashboard:
```
istioctl dashboard kiali
```
Select **hipster-app** namespace **Graph** on the left menu then select **Versioned App Graph**, ** Request Poucentage ** as Options:

![Kiali Traffic Splitting](/images/kiali-traffic-splitting-1.png?width=30pc)

![Kiali Traffic Splitting](/images/kiali-traffic-splitting-2.png?width=30pc)

On the **Display menu**, check  **Display menu**:

![Kiali Traffic Splitting](/images/kiali-traffic-splitting-3.png?width=10pc)

Graph will show the real traffic poucentage between the different versions:

![Kiali Traffic Splitting](/images/kiali-traffic-splitting-4.png?width=50pc)



## Exercise:
Traffic Shifting.
