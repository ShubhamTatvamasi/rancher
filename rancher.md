# Rancher


Add Rancher helm repo:
```bash
helm repo add rancher https://releases.rancher.com/server-charts/stable
helm repo update
```

Install Rancher:
```bash
helm install rancher rancher/rancher \
  --version 2.7.0 \
  --create-namespace \
  --namespace cattle-system \
  --set replicas=1 \
  --set hostname=rancher.shubhamtatvamasi.com \
  --set tls=external \
  --set ingress.includeDefaultExtraAnnotations=false \
  --set ingress.extraAnnotations."kubernetes\.io/ingress\.class"=caddy
```
