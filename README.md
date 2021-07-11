# rancher

### RKE

Setup Node
```bash
# Disable swap:
sudo swapoff -a

# Install docker for rancher:
curl https://releases.rancher.com/install-docker/20.10.sh | sh
```

https://rancher.com/docs/rancher/v2.x/en/installation/requirements/installing-docker/

https://github.com/rancher/rke/releases/tag/v1.2.9

Download rke for Ubuntu:
```bash
sudo wget https://github.com/rancher/rke/releases/download/v1.2.9/rke_linux-amd64 \
  -O /usr/local/bin/rke

sudo chmod +x /usr/local/bin/rke
```

Generate config for RKE cluster:
```bash
rke config
```

Setup cluster:
```bash
rke up
```

Merge rancher config file:
```bash
# Make a copy of your existing config
cp ~/.kube/config ~/.kube/config.bak

# Merge the two config files together into a new config file
KUBECONFIG=~/.kube/config:${PWD}/kube_config_cluster.yml kubectl config view --flatten > /tmp/config

# Replace your old config with the new merged config
mv /tmp/config ~/.kube/config

# (optional) Delete the backup once you confirm everything worked ok
rm ~/.kube/config.bak

# rename context of rancher cluster
kubectx rancher-cluster=local
```
---

### Rancher

#### Docker setup

Install rancher:
```bash
docker run -d --restart=unless-stopped \
  --name rancher \
  -p 80:80 -p 443:443 \
  -v /opt/rancher:/var/lib/rancher \
  rancher/rancher:v2.5.5 \
  --acme-domain \
  rancher.shubhamtatvamasi.com
```

#### Kubernetes Setup

Add Rancher and Nginx Ingress helm repo:
```bash
helm repo add nginx-stable https://helm.nginx.com/stable
helm repo add rancher-stable https://releases.rancher.com/server-charts/stable
helm repo update
```

Delete default ingress controller:
```bash
kubectl delete ns ingress-nginx
```

Install cert-manager:
```bash
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.1.0/cert-manager.yaml
```

Install metallb:
```bash
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/namespace.yaml
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.9.5/manifests/metallb.yaml

kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```

Setup IP range. Giving only 1 IP which is of the node IP
```bash
kubectl create -f - << EOF
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: address-pool-1
      protocol: layer2
      addresses:
      - $(kubectl get nodes -o jsonpath='{ $.items[*].status.addresses[?(@.type=="InternalIP")].address }')/32
EOF
```
> type can also be `ExternalIP` based on your node

create namespace
```bash
# Create new namespace
kubectl create namespace nginx-ingress

helm install nginx-ingress nginx-stable/nginx-ingress \
  --namespace nginx-ingress \
  --version=0.6.1
```

Install rancher:
```bash
# Create new namespace
kubectl create namespace cattle-system

helm install rancher rancher-stable/rancher \
  --namespace cattle-system \
  --set hostname=rancher.shubhamtatvamasi.com \
  --set ingress.tls.source=letsEncrypt \
  --set letsEncrypt.email=info@shubhamtatvamasi.com \
  --version=2.5.5
```

Enable websocket for rancher ingress:
```bash
kubectl patch ingress rancher -n cattle-system \
  --patch='{
    "metadata": {
      "annotations": {
        "nginx.org/websocket-services": "rancher"
      }
    }
  }'
```

Access Ingernal Services this way:

https://rancher.shubhamtatvamasi.com/api/v1/namespaces/default/services/http:nginx:80/proxy/

