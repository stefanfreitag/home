---
title: 'kind'
author: Stefan Freitag
---

### Installation

```shell
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/
sudo chown root:root /usr/local/bin/kind
```

### Creating a cluster

```bash
$ kind create cluster
Creating cluster "kind" ...
 â Ensuring node image (kindest/node:v1.18.2) đŧ
 â Preparing nodes đĻ
 â Writing configuration đ
 â Starting control-plane đšī¸
 â Installing CNI đ
 â Installing StorageClass đž
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind
```

- Create a cluster using configuration file

```bash
kind create cluster --config kind-example-config.yaml
```

kubectl cluster-info --context kind-kind
