---
title: "Configure DNS entry"
chapter: true
weight: 3
---
# Configure Google DNS

In the previous section we had configure our domain to be resolved by **[GCP Cloud DNS](https://cloud.google.com/dns/)**.

In this section we will configure Cloud DNS to assign the domain to the Kubernetes Cluster load balancer.

1. On Google Cloud dashboard menu, select **Network services → Cloud DNS**:
![GCP Cloud DNS](/images/gcp-cloud-dns.png?width=50pc)

2. Click on **Create zone**:
![GCP Cloud DNS create zone](/images/gcp-cloud-dns-create-zone.png?width=50pc)

3. Create the zone with:

  - Set **Zone type** to _public_
  - **DNS name**: enter the dns name. for the purpose of the workshop we will the Domain `innovlabs.io`
  - **Zone name**: use the training account as a name or select you cluster name (for example: `trainee001-srecon19`)
  - Set **DNSSEC** to _ON_
  the click on **Create**
  ![GCP Cloud DNS create zone](/images/gcp-cloud-dns-create-zone-2.png?width=50pc)

4. The created DNS zone details should be displayed:
![GCP Cloud DNS created](/images/gcp-cloud-dns-created-1.png?width=50pc)

5. We will add a record set for the hispter e-commerce app, click on **Add record set**:
  - Enter _hipster_ for **DNS Name**
  - Select _A_ from the **Resource Record Type** menu and set **TTL** to 1 minute.
  - Enter  the IP of the Load Balancer previously created for hipster app : `kubectl get service frontend-external` for **IPv4 Address**.
  then Click on **Create**:
   ![GCP Cloud DNS created](/images/gcp-cloud-dns-create-record.png?width=40pc)
  the DNS zone will be updated with the new Record:
   ![GCP Cloud DNS created](/images/gcp-cloud-dns-record-created.png?width=40pc)

6. wait a minute to get DNS cache updated and then open the application:
```
http://YOUR-APPLICATION-DOMAIN/
```

![GCP Cloud DNS created](/images/application-dns.png?width=40pc)
