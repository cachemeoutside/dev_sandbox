# Steps

* Create k3d cluster with LB
k3d cluster create argocd --api-port 6550 -p "34000:80@loadbalancer" --agents 2

* Create k3d cluster with LB and PV
k3d cluster create argocd --api-port 6550 -p "34000:80@loadbalancer" --agents 2 --volume ${PWD}/pv:/tmp/pv

* Hosts file contains
  127.0.0.1 argocd.local

* Install ArgoCD using values file located in this dir
  helm repo add argo https://argoproj.github.io/argo-helm
  helm install --create-namespace -n argocd argocd argo/argo-cd -f argocd.local-values.yaml

* Sandbox creds
  admin/admin

* Create app-of-apps helm chart for itself
  helm upgrade --install -n argocd argocd-apps argo/argocd-apps -f argocd.local-app-of-apps-values.yaml
