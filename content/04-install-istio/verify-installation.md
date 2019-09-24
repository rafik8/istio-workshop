---
title: "Verify the Installation"
chapter: true
weight: 3
---


Check the helm chart is deployed:
```
helm ls
```
```
NAME      	REVISION	UPDATED                 	STATUS  	CHART           	APP VERSION	NAMESPACE   
istio     	1       	Sat Aug 31 19:05:59 2019	DEPLOYED	istio-1.2.5     	1.2.5      	istio-system
istio-cni 	1       	Sat Aug 31 17:23:26 2019	DEPLOYED	istio-cni-0.1.0 	0.1.0      	kube-system
istio-init	1       	Sat Aug 31 15:54:08 2019	DEPLOYED	istio-init-1.2.5	1.2.5      	istio-system
```



You can verify that the services have been deployed using:


```
kubectl get svc -n istio-system
```
```
NAME                     TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                                                                                                                                      AGE
grafana                  ClusterIP      10.19.254.207   <none>        3000/TCP                                                                                                                                     3h37m
istio-citadel            ClusterIP      10.19.241.199   <none>        8060/TCP,15014/TCP                                                                                                                           3h37m
istio-galley             ClusterIP      10.19.246.165   <none>        443/TCP,15014/TCP,9901/TCP                                                                                                                   3h37m
istio-ingressgateway     LoadBalancer   10.19.248.241   34.76.51.8    15020:31196/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:30892/TCP,15030:31192/TCP,15031:31307/TCP,15032:31678/TCP,15443:32172/TCP   3h37m
istio-pilot              ClusterIP      10.19.255.53    <none>        15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       3h37m
istio-policy             ClusterIP      10.19.242.17    <none>        9091/TCP,15004/TCP,15014/TCP                                                                                                                 3h37m
istio-sidecar-injector   ClusterIP      10.19.246.210   <none>        443/TCP                                                                                                                                      3h37m
istio-telemetry          ClusterIP      10.19.246.201   <none>        9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       3h37m
jaeger-agent             ClusterIP      None            <none>        5775/UDP,6831/UDP,6832/UDP                                                                                                                   3h37m
jaeger-collector         ClusterIP      10.19.240.84    <none>        14267/TCP,14268/TCP                                                                                                                          3h37m
jaeger-query             ClusterIP      10.19.250.3     <none>        16686/TCP                                                                                                                                    3h37m
kiali                    ClusterIP      10.19.247.172   <none>        20001/TCP                                                                                                                                    3h37m
prometheus               ClusterIP      10.19.252.228   <none>        9090/TCP                                                                                                                                     3h37m
tracing                  ClusterIP      10.19.247.89    <none>        80/TCP                                                                                                                                       3h37m
zipkin                   ClusterIP      10.19.242.206   <none>        9411/TCP                                                                                                                                     3h37m
```
and check the corresponding pods with:

```
kubectl get pods -n istio-system
```

```
NAME                                      READY   STATUS      RESTARTS   AGE
grafana-97fb6966d-bmzfx                   1/1     Running     0          3h37m
istio-citadel-76f9586b8b-vmf2w            1/1     Running     0          3h37m
istio-galley-78f65c8469-tblgk             1/1     Running     0          3h37m
istio-ingressgateway-677756fc66-pmfzx     1/1     Running     0          3h37m
istio-init-crd-11-md48c                   0/1     Completed   0          6h49m
istio-init-crd-12-wb6wl                   0/1     Completed   0          6h49m
istio-pilot-65cb5648bf-qrhnf              2/2     Running     0          3h37m
istio-policy-8596cc6554-kdbmk             2/2     Running     1          3h37m
istio-sidecar-injector-76f487845d-zgl4k   1/1     Running     0          3h37m
istio-telemetry-5c6b6d59f6-dq68g          2/2     Running     1          3h37m
istio-tracing-595796cf54-989fk            1/1     Running     0          3h37m
kiali-55fcfc86cc-jhxk6                    1/1     Running     0          3h37m
prometheus-5679cb4dcd-whsjp               1/1     Running     0          3h37m
```
