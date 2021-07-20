---
title: 'kind'
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
 ✓ Ensuring node image (kindest/node:v1.18.2) 🖼 
 ✓ Preparing nodes 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind
```

- Create a cluster using configuration file

```bash
kind create cluster --config kind-example-config.yaml
```

kubectl cluster-info --context kind-kind
