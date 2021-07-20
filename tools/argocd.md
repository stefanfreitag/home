
## Installation

```bash
❯ kubectl create namespace argocd
namespace/argocd created

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Temporary portforwarding

```bash
❯ kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Getting initial password for UI login

user name is admin

```bash
❯ kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2

argocd-server-69678b4f65-m4zhq
```
