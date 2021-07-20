---
title: 'kubectl'
---

## Installation

```shell
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.20.0/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```

## Commands

```shell
kubectl cluster-info --context kind-kind
Kubernetes master is running at https://127.0.0.1:37699
KubeDNS is running at https://127.0.0.1:37699/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```
