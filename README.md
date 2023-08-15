# gitops-tests

## Prerequisite

This setup assumes you have a local cluster rready to be used.

## Deployment

To deploy ArgoCD with this test setup, you need to configure the correct clusters.
We try to simultate multiple clusters using different namespaces and cluster names, all present on the `in-cluster`.

Firstly, create the argocd-manager ServiceAccount to allow ArgoCD to manage the fake clusters:
```bash
kubectl apply -n kube-system -f bootstrap/serviceaccounts/argocd-manager.yaml
```
Then, get the token that was created for the ServiceAccount:
```bash
kubectl get -n kube-system secret argocd-manager-token -o jsonpath='{.data.token}' | base64 --decode
```

Secondly, make a copy of `example.yaml` at `./services/argo-cd/base/clusters` and rename it to `private.yaml`. Update the cluster configuration file `private.yaml` with the values from your `~/.kube/config` file and the command above.

Then, deploy argocd to your local cluster

```bash
kubectl apply -n argocd -k ./services/argo-cd
```

## Bootstrap

At first, the [sync-projects](./bootstrap/sync/sync-projects.yaml) application must be applied manually.

Then, the [bootstrap](./bootstrap/sync/bootstrap.yaml) can be applied manually.

```bash
kubectl apply -n argocd -f bootstrap/sync/sync-projects.yaml
kubectl apply -n argocd -f bootstrap/sync/bootstrap.yaml
```
