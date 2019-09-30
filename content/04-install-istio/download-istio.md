---
title: "Download Istio"
chapter: true
weight: 1
---
# Download Istio

1. We will create working directory for the workshop that we will refer it by environment variable: `WORKSHOP_HOME`.

```
mkdir ~/istio-workshop && cd ~/istio-workshop
```
```
export WORKSHOP_HOME=~/istio-workshop
```

1. Set Istio version in an environment variable (we will use the latest stable version):

```
export ISTIO_VERSION=1.3.1
```

1. Download Istio:

```
curl -L https://git.io/getLatestIstio | sh -

```

1. add `istioctl` to your path:

```
export PATH="$PATH:$WORKSHOP_HOME/istio-$ISTIO_VERSION/bin"
```

1. Check `istioctl` version:

```
istioctl version --remote=false
```

```
1.3.1
```

## Anatomy of an Istio package:

Below an overview on an Istio release package:


![Istio package anatomy](/images/istio-package-anatomy.png?width=40pc  "Istio package anatomy")

We can categorise Istio release package into 4 sections:

- `bin/istioctl`: is the CLI for the Istio control plane similar to `kubectl`.

- `install`: contains various installation options for the different platform. For Kubernetes, official support for [Helm Chart](https://helm.sh) and [Operator Framework](https://github.com/operator-framework).

- `sample`: contains various samples to get started with Istio and embrace its features.

- `tools`: contains tooling for perf testing, etc ...

<!-- ```
istio-1.3.0/
├── bin
|   └── istioctl
├── install
│   ├── consul
│   ├── gcp
│   ├── kubernetes
|   |   ├── helm
│   │   └── operator
│   └── tools
│       ├── setupIstioVM.sh
│       └── setupMeshEx.sh
├── samples
│   ├── bookinfo
│   ├── certs
│   ├── custom-bootstrap
│   ├── external
│   ├── fortio
│   ├── health-check
│   ├── helloworld
│   ├── httpbin
│   ├── https
│   ├── kubernetes-blog
│   ├── rawvm
│   ├── sleep
│   ├── tcp-echo
│   └── websockets
└── tools
    ├── checker
    ├── docker-dev
    ├── githubContrib
    ├── hyperistio
    ├── istio-iptables
    ├── license
    ├── packaging
    └── vagrant
``` -->



<!-- // version can be different as istio gets upgraded
//cd istio-*

//sudo mv -v bin/istioctl /usr/local/bin/ -->
