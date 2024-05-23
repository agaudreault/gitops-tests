
# Manifest Hydration

To hydrate the manifests in this repository, run the following commands:

```shell

git clone https://github.com/agaudreault/gitops-tests
# cd into the cloned directory
git checkout ca2ff0b38fae3dd02444d3615651129f1e394aba
helm dependency build
helm template . --name-template simple-app-east --namespace production-2 --values /tmp/_argocd-repo/dbb92269-9c18-47ad-b229-ae14b41dd16e/services/simple-application/values.yaml --values /tmp/_argocd-repo/dbb92269-9c18-47ad-b229-ae14b41dd16e/services/simple-application/prod/values.yaml --include-crds
```