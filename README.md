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
