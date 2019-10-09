---
title: "Ingress Gateway"
chapter: true
weight: 1
---

## Introduction

An Istio Gateway describes a load balancer operating at the edge of the mesh receiving incoming or outgoing HTTP/TCP connections. The specification describes a set of ports that should be exposed, the type of protocol to use, virtual host name to listen to, etc.

Ingress enables expose services to the external world and thus it is the entry point for all service running within the mesh.


Istio Gateway is based on envoy proxy, it handle reverse proxy and load balancing for services running in the service mesh network.

![Istio Ingress diagram](/images/istio-ingress.png?width=40pc)

## Istio Gateway vs Kubernetes Gateway

Unlike [Kubernetes Ingress Resources](https://kubernetes.io/docs/concepts/services-networking/ingress/), Istio Ingress does not include any traffic routing configuration. Traffic routing for ingress traffic is instead configured using Istio routing rules, exactly in the same was as for internal service requests.

## How it works

The Ingress Resource is is handled by two Istio Resources:

| Resource         | API                 | Version    |
| -----------------| --------------------|----------- |
| Gateway          | networking.istio.io | v1alpha3   |
| VirtualService   | networking.istio.io | v1alpha3   |


**Gateway**: The [Gateway](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/) resource is used to configure hosts exposed by the Gateway.

Valid protocols are:**HTTP|HTTPS|GRPC|HTTP2|MONGO|TCP|TLS**. More info about Gateways can be found in the [Istio Gateway docs](https://istio.io/docs/reference/config/networking/v1alpha3/gateway/).

**Virtual Service**:
[VirtualService](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/) works in tandem with the Gateway. it defines the destination service.
A Virtual Service defines the rules that control how requests for a service are routed within an Istio service mesh (Mesh Network). For example, a virtual service can route requests to different versions of a service or to a completely different service than was requested. Requests can be routed based on the request source and destination, HTTP paths and header fields, and weights associated with individual service versions.


![Istio ingress diagram](/images/istio-ingress-diagram.png)


{{% notice note %}}
When a VirtualService is configured with a Gateway resource using `gateways` field. This is means that the  service is exposed to outside of the mesh network. If not the service is mesh-wide only.
{{% /notice%}}

## Define an Ingress resource

Actually, The Hispter commerce application is exposed to external world using a Kubernetes Controller LoadBalancer provided by the Cloud Provider:

1.  Delete the LoadBalancer service:
```
kubectl delete service frontend-external
```

1. First, we need to find the Istio Ingress IP address:
```
export INGRESS_IP=$(kubectl get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}' -n istio-system)
echo $INGRESS_IP
```


1. Connect to the Istio Ingress IP:

```
curl $INGRESS_IP
```

```
curl: (7) Failed to connect to 35.187.106.238 port 80: Connection refused
```

The connection is refused because no service is running behind the ingress gateway.

Let's create a Gateway to the Hipster application so the application will be exposed to the external world through the Mesh Ingress Gateway but before that let's configure Cloud DNS to associate our domain name to the Ingress Gateway:


1. On Google Cloud dashboard menu, select **Network services → Cloud DNS** then edit the zone created previously:
![Cloud DNS update ingress](/images/cloud-dns-update-ingress.png?width=50pc)
Update the IPv4 Address with the value of `$INGRESS_IP` then click **save**.

1. We need to create a Gateway resource and Virtual Service:
  {{% notice info %}}
  Please change the host name in `$WORKSHOP_HOME/istio-workshop-labs/frontend-ingress.yaml` with your own before running the command.
  {{% /notice%}}
  ```
  kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/frontend-ingress.yaml
  ```

1. Check that the gateway and and the virtual service are created:
```
kubectl get virtualservice,gateway
```

```
NAME                                         GATEWAYS        HOSTS                                        AGE
virtualservice.networking.istio.io/hispter   [app-gateway]   [hipster.trainee001-srecon19.innovlabs.io]   5h

NAME                                      AGE
gateway.networking.istio.io/app-gateway   5h
```

1. Check the application on the browser using the configured host:

![Hipster app ingress](/images/hipster-app-ingress.png?width=50pc)


## Exercise:

let's assume that we want to expose Istio dashbaord using Ingress Gateway as following:

- dashboard.**your-domain**-srecon19.innovlabs.io/kiali → Kiali

- tracing.**your-domain**-srecon19.innovlabs.io → Jaeger Tracing


Create a YAML file that create an ingress resource for one of these Addons and deploy it to the mesh.

{{% notice tip %}}
Don't forget the prefix ;)
{{% /notice%}}
