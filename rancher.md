# Rancher


Add Rancher helm repo:
```bash
helm repo add rancher https://releases.rancher.com/server-charts/stable
```

Install Rancher:
```bash
helm install rancher rancher/rancher \
  --create-namespace \
  --namespace cattle-system \
  --set hostname=rancher.shubhamtatvamasi.com
```
