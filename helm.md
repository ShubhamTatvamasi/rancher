# Helm


1. Pull upstream rancher 2.14.2 and unpack
```
cd /tmp
helm pull rancher --repo https://releases.rancher.com/server-charts/stable --version 2.14.2 --untar
cd rancher
```

2. Loosen the kubeVersion cap (< 1.36.0-0  ->  < 1.37.0-0)
```bash
sed -i '' 's/kubeVersion:.*/kubeVersion: < 1.37.0-0/' Chart.yaml
grep kubeVersion Chart.yaml      # verify it now reads: kubeVersion: < 1.37.0-0
```

3. Repackage (stays version 2.14.2)
```bash
cd ..
helm package rancher            # -> rancher-2.14.2.tgz
```

4. Log in to Harbor and push as an OCI chart
```bash
helm registry login harbor.k8s.shubhamtatvamasi.com          # use your Harbor user/pass
helm push rancher-2.14.2.tgz oci://harbor.k8s.shubhamtatvamasi.com/charts
```
> create `charts` project on Harbor before pushing.
