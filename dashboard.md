# Dashboard

Add helm repo:
```bash
helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
helm repo update
```

Install Rancher:
```bash
helm upgrade -i rancher rancher-stable/rancher \
  --create-namespace \
  --namespace cattle-system \
  --set hostname=rancher.magma.shubhamtatvamasi.com \
  --set ingress.includeDefaultExtraAnnotations=false \
  --set ingress.tls.source=secret \
  --set ingress.ingressClassName=caddy
```
