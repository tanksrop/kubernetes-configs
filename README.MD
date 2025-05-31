# Kubernetes-configs

## k3s setup

## set secrets for bitwarden

```
kubectl create secret generic bitwarden-cli \
  --from-literal=BW_CLIENTID='your-client-id' \
  --from-literal=BW_CLIENTSECRET='your-client-secret' \
  --from-literal=BW_PASSWORD='your-bitwarden-password' \
  --from-literal=BW_HOST='https://bitwarden.com' \
  -n your-namespace
```

## argocd setup

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo

kubectl port-forward svc/argocd-server -n argocd 8080:443

Open https://localhost:8080 in your browser.

username: admin
password: *argocd-initial-admin-secret*