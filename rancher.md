# Rancher


Add Rancher helm repo:
```bash
helm repo add rancher https://releases.rancher.com/server-charts/stable
```

Install Rancher:
```bash
helm install rancher rancher-stable/rancher \
  --namespace cattle-system \
  --set hostname=rancher.shubhamtatvamasi.com \
  --set ingress.tls.source=letsEncrypt \
  --set letsEncrypt.email=info@shubhamtatvamasi.com \
  --version=2.5.5
```
