
# Manifest Hydration

To hydrate the manifests in this repository, run the following commands:

```shell

git clone https://github.com/agaudreault/gitops-tests
# cd into the cloned directory
git checkout 61d5c337ea259389cc3ebfa0e449317a3517be42
helm dependency build
helm template . --name-template staging-east --include-crds
```