---
title: "Verify deployment"
chapter: true
weight: 3
---


## Access Application:

1. Find the IP address of your application, then visit the application on your browser to confirm installation:

```
kubectl get service frontend-external
```

```
NAME                TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)        AGE
frontend-external   LoadBalancer   10.19.244.242   146.148.112.234   80:31937/TCP   45h
```

2. Visit http://EXTERNAL-IP/ and the application home page should be rendered:

![Hipster Shop Demo](/images/application-view.png?width=50pc)



{{% notice note %}}
We are exposing the frontend application using service of type Load LoadBalancer. Lately, we will changed to ClusterIP and use the _Service Mesh Ingress Gateway_ to expose the service outside of the cluster.
[kubernetes-manifests/frontend.yaml#L89](https://github.com/GoogleCloudPlatform/microservices-demo/blob/master/kubernetes-manifests/frontend.yaml#L89)


{{< highlight yaml >}}
apiVersion: v1
kind: Service
metadata:
  name: frontend-external
spec:
  type: LoadBalancer
  selector:
    app: frontend
  ports:
  - name: http
    port: 80
    targetPort: 8080
{{< /highlight >}}

{{% /notice%}}
