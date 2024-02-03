# flux-system

Each `crd` bundle is in dedicated folder with corresponding `kustomize`. Other `kustomize`s can declare dependencies on those. 
All flux manifests and repository configs are in `./components` and in base folder are `kustomize`s for each namespace wich configured dependencies.

To initialize or upgrade flux:

```bash
curl -L https://github.com/fluxcd/flux2/releases/download/v2.2.2/flux_2.2.2_linux_amd64.tar.gz \
    | tar -zx flux && sudo mv ./flux /usr/local/bin/flux \
  && flux --version  # bin/flux https://github.com/fluxcd/flux2/releases

kubectl krew install slice


flux install --export \
  > ./flux-system-temp.yaml

kubectl slice --input-file ./flux-system-temp \
  --include-kind CustomResourceDefinition \
  --output-dir ./crds --template '{{.metadata.name}}.yaml'

kubectl slice --input-file ./flux-system-temp \
  --exclude-kind CustomResourceDefinition \
  --stdout > ./components/flux-components-minus-crds.yaml

rm ./flux-system-temp


# verify ./kustomization.yaml
# and push to cluster unless already initialized

kubectl create -k .
```
