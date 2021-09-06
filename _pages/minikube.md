---
title: minikube
author: Stefan Freitag
---
## Installation

- Download the executable

  ```shell
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  sudo mv minikube-linux-amd64 /usr/local/bin/minikube
  sudo chown root: /usr/local/bin/minikube
  sudo chmod +x /usr/local/bin/minikube
  ```

- Check the version

  ```shell
  minikube version
  minikube version: v1.22.0
  commit: a03fbcf166e6f74ef224d4a63be4277d017bb62e
  ```

## Commands

- Retrieves the IP address of the specified node

  ```shell
  minikube ip
  192.168.58.2
  ```

