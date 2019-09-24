---
title: "Deploy application"
chapter: true
weight: 2
---


## Install Skaffold:

```
mkdir skaffold && cd skaffold
curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-darwin-amd64
sudo chmod +x skaffold
cd ..
export PATH="$PATH:/technical/github/istio-workshop/releases/skaffold/"
```


Check that skaffold is installed:
```
skaffold version
v0.37.0
```
 //TOOD: Add illustration schema fo skaffold




{{% notice tip %}}
if you are using Google Cloud console, you haven't to deploy skaffold. it is alredy coming within the VM.
{{% /notice%}}

## Deploy

1. Enable Google Container Registry (GCR) on your GCP project and configure the docker CLI to authenticate to GCR:

```
gcloud services enable containerregistry.googleapis.com
```

```
gcloud auth configure-docker -q
```
You should a confirmation that Docker configuration file updated.


## Application namespace

1. We will create
```
kubectl create namespace hipster-app
```

2. Switch the context to the created namespace:

```
kubectl config set-context --current --namespace=hipster-app
```


## Build image and Deploy with Skaffold

1. Clone docker repository
```
git clone https://github.com/GoogleCloudPlatform/microservices-demo.git
cd microservices-demo
```

2. In the root of this repository, run the following command:
//run skaffold run --default-repo=gcr.io/[PROJECT_ID], where [PROJECT_ID] is your GCP project ID.

```
cd microservices-demo/
skaffold run --default-repo=gcr.io/$PROJECT_ID
```

```
Generating tags...
 - gcr.io/srecon19-workshop-250603/emailservice -> gcr.io/srecon19-workshop-250603/emailservice:v0.1.2
 - gcr.io/srecon19-workshop-250603/productcatalogservice -> gcr.io/srecon19-workshop-250603/productcatalogservice:v0.1.2
 - gcr.io/srecon19-workshop-250603/recommendationservice -> gcr.io/srecon19-workshop-250603/recommendationservice:v0.1.2
 - gcr.io/srecon19-workshop-250603/shippingservice -> gcr.io/srecon19-workshop-250603/shippingservice:v0.1.2
 - gcr.io/srecon19-workshop-250603/checkoutservice -> gcr.io/srecon19-workshop-250603/checkoutservice:v0.1.2
 - gcr.io/srecon19-workshop-250603/paymentservice -> gcr.io/srecon19-workshop-250603/paymentservice:v0.1.2
 - gcr.io/srecon19-workshop-250603/currencyservice -> gcr.io/srecon19-workshop-250603/currencyservice:v0.1.2
 - gcr.io/srecon19-workshop-250603/cartservice -> gcr.io/srecon19-workshop-250603/cartservice:v0.1.2
 - gcr.io/srecon19-workshop-250603/frontend -> gcr.io/srecon19-workshop-250603/frontend:v0.1.2
 - gcr.io/srecon19-workshop-250603/loadgenerator -> gcr.io/srecon19-workshop-250603/loadgenerator:v0.1.2
 - gcr.io/srecon19-workshop-250603/adservice -> gcr.io/srecon19-workshop-250603/adservice:v0.1.2
Tags generated in 364.368285ms
...
```

**Note for presenter**: this will take almost % min. you need to present the next topic meanwhile.

This command:

- builds the container images
- pushes them to GCR
- applies the ./kubernetes-manifests deploying the application to Kubernetes.

{{% notice note %}}
if you do not have docker installed locally, run skaffold with `-p gcp` option. This allows building and pushing the images on Google Container Builder without requiring docker installed on the developer machine.
```
gcloud services enable cloudbuild.googleapis.com
skaffold run -p gcb --default-repo=gcr.io/$PROJECT_ID
```
{{% /notice%}}



```
DONE
Build complete in 4m42.50637363s
Starting test...
Test complete in 3.521µs
Starting deploy...
kubectl client version: 1.13
deployment.apps/adservice created
service/adservice created
deployment.apps/cartservice created
service/cartservice created
deployment.apps/checkoutservice created
service/checkoutservice created
deployment.apps/currencyservice created
service/currencyservice created
deployment.apps/emailservice created
service/emailservice created
deployment.apps/frontend created
service/frontend created
service/frontend-external created
deployment.apps/loadgenerator created
deployment.apps/paymentservice created
service/paymentservice created
deployment.apps/productcatalogservice created
service/productcatalogservice created
deployment.apps/recommendationservice created
service/recommendationservice created
deployment.apps/redis-cart created
service/redis-cart created
deployment.apps/shippingservice created
service/shippingservice created
Deploy complete in 10.084532421s
You can also run [skaffold run --tail] to get the logs
```

Check that all pods are deployed:

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


{{% notice note %}}
if you get an error during the build, please re-run the command.
{{% /notice%}}
