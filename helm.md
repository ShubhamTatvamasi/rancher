# Helm

https://artifacthub.io/packages/helm/rancher-stable/rancher

1. Pull upstream rancher 2.14.2 and unpack
```
cd /tmp

helm pull rancher \
  --repo https://releases.rancher.com/server-charts/stable \
  --version 2.14.2 \
  --untar

cd rancher
```

2. Loosen the kubeVersion cap (< 1.36.0-0  ->  < 1.37.0-0)
```bash
sed -i '' 's#^kubeVersion: < 1.36.0-0#kubeVersion: < 1.37.0-0#' Chart.yaml

sed -i '' 's#{{- printf "%s-%s" .Chart.Name .Chart.Version | trunc 63 | trimSuffix "-" -}}#{{- printf "%s-%s" .Chart.Name (regexReplaceAll "[+].*$" (.Chart.Version | toString) "") | trunc 63 | trimSuffix "-" -}}#' templates/_helpers.tpl
```

3. Repackage (stays version 2.14.2)
```bash
cd ..
helm package rancher
```

4. Log in to Harbor and push as an OCI chart
```bash
helm registry login harbor.k8s.shubhamtatvamasi.com          # use your Harbor user/pass
helm push rancher-2.14.2.tgz oci://harbor.k8s.shubhamtatvamasi.com/charts
```
> create `charts` project on Harbor before pushing.
