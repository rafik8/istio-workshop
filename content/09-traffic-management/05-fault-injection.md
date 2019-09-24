---
title: "Fault Injection"
chapter: true
weight: 5
---
# Fault Injection


## The pattern

Istio enables fault injection to test the resiliency of your application. With this feature, you can use application-layer fault injection instead of killing pods, delaying packets, or corrupting packets at the TCP layer.
Istio defines two types of faults injection:
**Delays**: Delays are timing failures such us network latency or overloaded upstreams.

**Aborts**: Aborts are crash failures such as HTTP error codes or TCP connection failures.


## How it works


The traffic splitting is handled by the following Istio Object:


| Object           | API                 | Version    |
| -----------------| --------------------|----------- |
| VirtualService   | networking.istio.io | v1alpha3   |


### Delay injection

Scenario: we want test the availability of the whole application when there is a delay `Recommendation Service`. We define a VirtualService for the target service and we set `fault` of type `delay` with these two attributes of the http request.

This below rule will inject a fixed 5 seconds delay on all the requests going to `Recommendation Service`. You can modify `percent` and `fixedDelay` to change the probability and the amount of time for delay.

- fixedDelay:
- percent: poucentage of the requests to apply the fault deloy.

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: recommendationservice-delay
spec:
  hosts:
  - recommendationservice
  http:
    - route:
        - destination:
            host: recommendationservice
      fault:
        delay:
          percent: 100
          fixedDelay: 5s
```

1. Open the hipster application on the browser and choose click to the one the products, product page details will trigger `Recommendation Service`. Check the page loading time:



1. Create a fault injection rule:


```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/recommendationservice-vs-delay.yaml
```

2. Refresh the page or select any other product details page and you’ll see that loading the page takes roughly 5s.


![Delay injection](/images/fault-injection-delay-recommandation.png?width=70pc)


### Fault injection

This is a second example configure istio myservice to return 503 for each received request. the recommandation service will be out of the service and we would like to test the application when we have an outage service.
Such configuration is useful  when we want to isolate a service for analyse or because of high security leak impacting the service.

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: myservice-503
spec:
  hosts:
    - myservice
  http:
    - route:
        - destination:
            host: myservice
      fault:
        abort:
          percent: 100
          httpStatus: 503
```

1. Delete the delay rule:

```
kubectl delete -f $WORKSHOP_HOME/istio-workshop-labs/recommendationservice-vs-delay.yaml
```

2. Create a fault injection rule:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/recommendationservice-vs-503.yaml
```

3. Go to the Hispter application then click on a product from the list to get his details, we will get the below error on the screen:

![Fault injection](/images/fault-injection-503-recommandation.png?width=70pc)



We can see that the Product details page will not available when `Recommendation service` is returning 503.

{{% notice note %}}
As you can see, Istio can help you to harden your microservices architecture by providing handy tools to simulate different kind of application error cases.
{{% /notice%}}


## Exercise

1. Change the abort percent to 20.

2. Open the application on the browser and check that you will get the error once after 4 product details requests:

2. Open Kiali and check the percent of 503 messages:

![Delay injection](/images/fault-injection-503-kiali.png?width=70pc)
