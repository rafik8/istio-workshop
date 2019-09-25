---
title: "Download Istio"
chapter: true
weight: 1
---
# Download Istio

1. We will create working directory for the workshop that we will refer it by environment variable: `WORKSHOP_HOME`.

```
mkdir istio-workshop && cd istio-workshop

export WORKSHOP_HOME=/path-to/istio-workshop
```

1. Export Istio version (we will use the latest stable version):

```
export ISTIO_VERSION=1.3.0
```

1. Download Istio:

```
curl -L https://git.io/getLatestIstio | sh -

// version can be different as istio gets upgraded
//cd istio-*

//sudo mv -v bin/istioctl /usr/local/bin/

```

1. add `istioctl` to your environment path variable with:

```
export PATH="$PATH:/your-path/istio-workshop/releases/istio-1.3.0/bin"
```

1. Check `istioctl` version:

```
istioctl version --remote=false
```

```
1.3.0
```
