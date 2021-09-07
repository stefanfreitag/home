---
title: 'helm'
author: Stefan Freitag
---

## Installation on Ubuntu 20.04

```bash
$ curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  6815  100  6815    0     0  30022      0 --:--:-- --:--:-- --:--:-- 30022
Downloading https://get.helm.sh/helm-v3.3.0-linux-amd64.tar.gz
Preparing to install helm into /usr/local/bin
[sudo] Passwort für stefan:
helm installed into /usr/local/bin/helm
```

## Commands

- Create a new helm chart

  ```bash
  helm create <chart-name>
  ```

- Package a chart

  ```bash
  helm package <directory>
  Successfully packaged chart and saved it to: [...]
  ```
  
- Search for helm charts

  ```bash
  helm search hub wordpress
  ```

- Add stable repository

  ```bash
  helm repo add stable https://kubernetes-charts.storage.googleapis.com
  ```

- Update repositories

  ```bash
  helm repo update
  ```

Example grafana:

```shell
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-release bitnami/grafana
```

Example consul

```bash
helm install consul hashicorp/consul --set global.name=consul
kubectl port-forward service/consul-server 8500:8500
```
