# rancher

### RKE

Setup Node
```bash
# Disable swap:
sudo swapoff -a

# Install docker for rancher:
curl https://releases.rancher.com/install-docker/24.0.sh | sh

sudo usermod -aG docker $USER
```

Increase pod limit:
```yaml
services:
  kubelet:
    extra_args:
      max-pods: 250
```

https://rancher.com/docs/rancher/v2.6/en/installation/requirements/installing-docker/ \
https://github.com/rancher/rke/releases

Download rke for Ubuntu:
```bash
sudo wget https://github.com/rancher/rke/releases/download/v1.3.0/rke_linux-amd64 \
  -O /usr/local/bin/rke

sudo chmod +x /usr/local/bin/rke
```

Generate config for RKE cluster:
```bash
rke config -a
```

Setup cluster:
```bash
rke up
```

Update `cluster.yml` for adding or removing nodes, then run:
```bash
rke up --update-only
```

Rotate RKE cluster certificates:
```bash
rke cert rotate
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

### RKE Upgrade – Change

- Take an etcd snapshot
    ```
    rke etcd snapshot-save --config cluster.yml --name pre-k8s-upgrade-`date '+%Y%m%d%H%M%S'`
    ```
- Change `kubernetes_version` in the `cluster.yml`
    ```
    kubernetes_version: "1.19.7-rancher1-1"
    ```

https://github.com/mattmattox/Kubernetes-Master-Class-Upgrades
