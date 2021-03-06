---
title: 'kubectl'
author: Stefan Freitag
tags: 
 - k8s 
 - kubernetes 
 - kubectl

---

## Installation

```shell
curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.20.0/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```

## Commands

- Display cluster information

  ```shell
  $ kubectl cluster-info --context kind-kind
  Kubernetes master is running at https://127.0.0.1:37699
  KubeDNS is running at https://127.0.0.1:37699/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
  To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
  ```

- List everything

  ```shell
  kubectl get all
  ```

### Pods

- Creating a pod from a file

  ```shell
  kubectl apply -f <pod_definition>.yaml
  ```

- Run a command inside a pod

  ```shell
  kubectl exec <pod_name> ls
  ```

- Login to a pod

  ```shell
  kubectl -it exec <pod_name> sh
  ```

- Show details of a pod

  ```shell
  kubectl describe pod <pod_name>
  ```

- Show pod labels

  ```shell
  kubectl get pods --show-labels
  ```

- Deleting a pod

  ```shell
  kubectl delete pods <pod_name>
  ```

## Services

## Deployments

- Show deployment status

  ```shell
  kubectl rollout status deployment <deployment_name>
  deployment "webapp" successfully rolled out
  ```

- Show deployment history
  
  ```shell
  kubectl rollout history deployment <deployment_name>
  ```

- Rollback to previous deployment version

  ```shell
  kubectl rollout undo deployment <deployment_name>
  ```
