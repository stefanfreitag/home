---
title: 'kompose'
author: Stefan Freitag
---

kompose is a tool to help users who are familiar with docker-compose move to Kubernetes. kompose takes a Docker Compose file and translates it into Kubernetes resources.

## Installation

- Download and place under `/usr/local/bin`

    ```shell
    curl -L https://github.com/kubernetes/kompose/releases/download/v1.24.0/kompose-linux-amd64 -o kompose
    chmod +x kompose
    chown root:root kompose
    sudo mv ./kompose /usr/local/bin/kompose
    ```

- Check version

    ```shell
    $ kompose version
    1.24.0 (4a2a0458)
    ```

## Links

- [GitHub](https://github.com/kubernetes/kompose)