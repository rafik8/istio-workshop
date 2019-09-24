---
title: "Provision DNS name"
chapter: true
weight: 2
---

# Provision DNS name

1. Click on **Sign In** and Enter you login and password.

2. Once connected, go through the quick tour and then click on **DNS zones → Add new**:

![Cloud DNS add new](/images/cloudns-add-new.png?width=50pc)


3. Select **Free Zone** to create a free DNS entry:

![Cloud DNS free zone](/images/cloudns-free-zone.png?width=50pc)

4. Enter the provided username if you are running this workshop as a part of [SRECON 19](https://srecon19emea.sched.com/event/Scjb/managing-microservices-with-istio-service-mesh) or choose your own if you are running the workshop outside of the event then click **REGISTER**.

![Cloud DNS domain registration](/images/cloudns-domain-registration.png?width=50pc)

5. After registration, you will be redirected to the domain dashboard:

![Cloud DNS domain dashboard](/images/cloudns-domain-dashboard.png?width=50pc)

4. You need to change  the NS entries and point to **Google Cloud DNS servers**:

```html
ns-cloud-c1.googledomains.com
ns-cloud-c2.googledomains.com
ns-cloud-c3.googledomains.com
ns-cloud-c4.googledomains.com
```

![Cloud DNS domain dashboard](/images/cloudns-domain-update.png?width=50pc)
