---
title: "Download Istio"
chapter: true
weight: 1
---
# Download Istio

1. Create releases directory:

```
mkdir releases && cd releases
```
2. Export Istio version (we will use the latest stable version):
```
export ISTIO_VERSION=1.3.0
```
3. Download Istio
```
curl -L https://git.io/getLatestIstio | sh -

// version can be different as istio gets upgraded
//cd istio-*

//sudo mv -v bin/istioctl /usr/local/bin/

```

4. add `istioctl` to your environment path variable with:

```
export PATH="$PATH:/your-path/istio-workshop/releases/istio-1.3.0/bin"
```

5. Check `istioctl` version:

```
istioctl version --remote=false
```

```shell
1.3.0
```

{{% notice note %}}
The error message is normal as we haven't yet install Istio and thus `istioctl` can not fetch Istio components versions.
{{% /notice%}}
