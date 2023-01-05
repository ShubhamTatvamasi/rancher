# Rancher


Add Rancher helm repo:
```bash
helm repo add rancher https://releases.rancher.com/server-charts/stable
helm repo update
```

Install Rancher:
```bash
helm install rancher rancher/rancher \
  --create-namespace \
  --namespace cattle-system \
  --set hostname=rancher.shubhamtatvamasi.com \
  --set ingress.includeDefaultExtraAnnotations=false \
  --set ingress.extraAnnotations."kubernetes.io/ingress.class"=caddy
```
