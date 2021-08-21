---
title: argocd
author: Stefan Freitag
classes: wide
---

## Installation

- Create a separate namespace `argocd`.

    ```sh
    $ kubectl create namespace argocd
    namespace/argocd created
    ```

- Run the installation script.

    ```sh
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    ```

- Enable temporary port forwarding

    ```sh
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```

- Getting initial password for UI login. The user name is `admin`

    ```sh
    $ kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2

    argocd-server-69678b4f65-m4zhq
    ```
