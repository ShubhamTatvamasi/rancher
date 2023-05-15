# Dashboard

Add Rancher helm repo:
```bash
helm repo add rancher https://releases.rancher.com/server-charts/stable
helm repo update
```

Install Rancher with nginx:
```bash
helm upgrade -i rancher rancher/rancher \
  --create-namespace \
  --namespace cattle-system \
  --set hostname=rancher.magma.shubhamtatvamasi.com \
  --set ingress.includeDefaultExtraAnnotations=false \
  --set ingress.tls.source=secret \
  --set ingress.ingressClassName=caddy
```

Install Rancher with Caddy:
```bash
helm upgrade -i rancher rancher/rancher \
  --version 2.7.1 \
  --create-namespace \
  --namespace cattle-system \
  --set replicas=1 \
  --set hostname=rancher.shubhamtatvamasi.com \
  --set tls=external \
  --set ingress.includeDefaultExtraAnnotations=false \
  --set ingress.extraAnnotations."kubernetes\.io/ingress\.class"=caddy
```

Get one time password:
```bash
kubectl get secret --namespace cattle-system bootstrap-secret \
  -o go-template='{{.data.bootstrapPassword|base64decode}}{{"\n"}}'
```

