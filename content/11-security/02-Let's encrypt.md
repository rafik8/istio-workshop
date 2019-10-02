---
title: "Securing Ingress Gateway with Let's Encrypt"
chapter: true
weight: 2
draft: false
---
# Securing Ingress Gateway with Let's Encrypt

## Introduction



## Configure Istio Gateway with Let's Encrypt certificate

1. Create a Istio Gateway in istio-system namespace with HTTPS redirect:

```
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: public-gateway
  namespace: istio-system
spec:
  selector:
    istio: ingressgateway
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "*"
    tls:
      httpsRedirect: true
  - port:
      number: 443
      name: https
      protocol: HTTPS
    hosts:
    - "*"
    tls:
      mode: SIMPLE
      privateKey: /etc/istio/ingressgateway-certs/tls.key
      serverCertificate: /etc/istio/ingressgateway-certs/tls.crt
```

1. Apply the resource to create the Gateway:

```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/istio-public-gateway.yaml
```

1. Create a service account with Cloud DNS admin role (replace project-id with your project ID):

```
gcloud iam service-accounts create dns-admin \
--display-name=dns-admin \
--project=${PROJECT_ID}
```

```
gcloud iam service-accounts keys create ./gcp-dns-admin.json \
--iam-account=dns-admin@${PROJECT_ID}.iam.gserviceaccount.com \
--project=${PROJECT_ID}
```

```
gcloud projects add-iam-policy-binding ${PROJECT_ID} \
--member=serviceAccount:dns-admin@${PROJECT_ID}.iam.gserviceaccount.com \
--role=roles/dns.admin
```

1. Create a Kubernetes secret with the GCP Cloud DNS admin key:

```
kubectl create secret generic cert-manager-credentials \
--from-file=./gcp-dns-admin.json \
--namespace=istio-system
```

1. Install cert-manager's CRDs:

```
CERT_REPO=https://raw.githubusercontent.com/jetstack/cert-manager

kubectl apply -f ${CERT_REPO}/release-0.7/deploy/manifests/00-crds.yaml
```

1. Create the cert-manager namespace and disable resource validation:

```
kubectl create namespace cert-manager

kubectl label namespace cert-manager certmanager.k8s.io/disable-validation=true
```

1. Install cert-manager with Helm:

```
helm repo add jetstack https://charts.jetstack.io && \
helm repo update && \
helm upgrade -i cert-manager \
--namespace cert-manager \
--version v0.7.0 \
jetstack/cert-manager
```

You should have a similar output message:
```
...
NOTES:
cert-manager has been deployed successfully!
...

```


1. Create a letsencrypt issuer for Cloud DNS (replace _trainee001-srecon19.innovlabs.io_ with a valid email address and project-id with your project ID):

```
apiVersion: certmanager.k8s.io/v1alpha1
kind: Issuer
metadata:
  name: letsencrypt-prod
  namespace: istio-system
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: trainee001.srecon19@innovsquare.com
    privateKeySecretRef:
      name: letsencrypt-prod
    dns01:
      providers:
      - name: cloud-dns
        clouddns:
          serviceAccountSecretRef:
            name: cert-manager-credentials
            key: gcp-dns-admin.json
          project: project-id
```

Then apply the configuration:
```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/letsencrypt-issuer.yaml
```

1. Create a certificate (replace _trainee001-srecon19.innovlabs.io_ with your domain):

```
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: istio-gateway
  namespace: istio-system
spec:
  secretName: istio-ingressgateway-certs
  issuerRef:
    name: letsencrypt-prod
  commonName: "*.trainee001-srecon19.innovlabs.io"
  acme:
    config:
    - dns01:
        provider: cloud-dns
      domains:
      - "*.trainee001-srecon19.innovlabs.io"
      - "trainee001-srecon19.innovlabs.io"
```

Then apply the configuration:
```
kubectl apply -f $WORKSHOP_HOME/istio-workshop-labs/letsencrypt-cert.yaml
```


1. Wait a couple of minutes and check that cert-manager fetched a wildcard certificate from letsencrypt.org:

```
kubectl -n istio-system describe certificate istio-gateway
```

```
Events:
  Type    Reason         Age    From          Message
  ----    ------         ----   ----          -------
  Normal  CertIssued     2m34s  cert-manager  Certificate issued successfully
```


```
Name:         istio-gateway
Namespace:    istio-system
Labels:       <none>
Annotations:  kubectl.kubernetes.io/last-applied-configuration:
                {"apiVersion":"certmanager.k8s.io/v1alpha1","kind":"Certificate","metadata":{"annotations":{},"name":"istio-gateway","namespace":"istio-sy...
API Version:  certmanager.k8s.io/v1alpha1
Kind:         Certificate
Metadata:
  Creation Timestamp:  2019-10-02T13:14:22Z
  Generation:          3
  Resource Version:    785388
  Self Link:           /apis/certmanager.k8s.io/v1alpha1/namespaces/istio-system/certificates/istio-gateway
  UID:                 8c88d880-e516-11e9-9128-42010a840062
Spec:
  Acme:
    Config:
      Dns 01:
        Provider:  cloud-dns
      Domains:
        *.trainee701-srecon19.innovlabs.io
        trainee701-srecon19.innovlabs.io
  Common Name:  *.trainee701-srecon19.innovlabs.io
  Issuer Ref:
    Name:       letsencrypt-prod
  Secret Name:  istio-ingressgateway-certs
Status:
  Conditions:
    Last Transition Time:  2019-10-02T13:14:22Z
    Message:               Certificate issuance in progress. Temporary certificate issued.
    Reason:                TemporaryCertificate
    Status:                False
    Type:                  Ready
Events:
  Type    Reason              Age   From          Message
  ----    ------              ----  ----          -------
  Normal  Generated           12s   cert-manager  Generated new private key
  Normal  GenerateSelfSigned  12s   cert-manager  Generated temporary self signed certificate
  Normal  OrderCreated        12s   cert-manager  Created Order resource "istio-gateway-523469874"
```


1. Recreate Istio ingress gateway pod:

```
kubectl delete pods -l istio=ingressgateway -n istio-system
```
