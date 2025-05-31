# Kubernetes-configs

## k3s setup

###install initial k3s control plane:
```
curl -sfL https://get.k3s.io | sh -
```
###Get the node-token 
```
sudo cat /var/lib/rancher/k3s/server/node-token
```
###To add additional control plane nodes, run:
```
curl -sfL https://get.k3s.io | \
  K3S_URL=https://<existing-server-ip>:6443 \
  K3S_TOKEN=<node-token> \
  INSTALL_K3S_EXEC="server" \
  sh -

```
###To add worker nodes, run:
```
curl -sfL https://get.k3s.io | \
  K3S_URL=https://<server-ip>:6443 \
  K3S_TOKEN=<node-token> \
  sh -
```

*optional but recommended*
install kubectl on your pc

Run the following on a k3s node
```
sudo cat /etc/rancher/k3s/k3s.yaml
```

create a kubeconfig file and copy the contents of /etc/rancher/k3s/k3s.yaml to it, changing the server address to that of a control plane,

## set secrets for bitwarden

```
kubectl create secret generic bitwarden-cli \
  --from-literal=BW_CLIENTID='your-client-id' \
  --from-literal=BW_CLIENTSECRET='your-client-secret' \
  --from-literal=BW_PASSWORD='your-bitwarden-password' \
  --from-literal=BW_HOST='https://bitwarden.com' \
  -n external-secrets
```

## argocd setup

kubectl create namespace argocd

```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Access ArgoCD webui
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open https://localhost:8080 in your browser.

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
```

username: admin
password: *argocd-initial-admin-secret*

