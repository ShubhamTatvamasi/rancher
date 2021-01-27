# rancher

https://github.com/rancher/rke/releases/tag/v1.2.5

Download rke for Ubuntu:
```bash
sudo wget https://github.com/rancher/rke/releases/download/v1.2.5/rke_linux-amd64 \
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

Merge config file:
```bash
# Make a copy of your existing config
$ cp ~/.kube/config ~/.kube/config.bak

# Merge the two config files together into a new config file
$ KUBECONFIG=~/.kube/config:${PWD}/kube_config_cluster.yml kubectl config view --flatten > /tmp/config

# Replace your old config with the new merged config
$ mv /tmp/config ~/.kube/config

# (optional) Delete the backup once you confirm everything worked ok
$ rm ~/.kube/config.bak
```

