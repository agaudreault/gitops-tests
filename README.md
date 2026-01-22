# gitops-tests

## Prerequisite

This setup assumes you have a local kind cluster ready to be used with the configuration below for the Kubernetes API endpoint.

```yaml
- cluster:
    insecure-skip-tls-verify: true
    server: https://host.docker.internal:<API_SERVER_PORT>
  name: kind-argo-local
```

## Deployment

To deploy ArgoCD with this test setup, you need to configure the correct clusters.
We try to simultate multiple clusters using different namespaces and cluster names, all present on the `in-cluster`.

For the config to work both locally when debugging ArgoCD and in our remote kind cluster (on MacOS),
we use `host.docker.internal` as the Kubernetes Service external name. You will need to update your local `/etc/hosts` file to add
the following:

```
127.0.0.1 host.docker.internal
127.0.0.1 staging.argocd.svc
127.0.0.1 dev.argocd.svc
127.0.0.1 production-1.argocd.svc
127.0.0.1 production-2.argocd.svc
```

Once configured, you can update the cluster configuration:

1. Create the argocd-manager ServiceAccount to allow ArgoCD to manage the fake clusters:

   ```bash
   kubectl apply -n kube-system -f bootstrap/serviceaccounts/argocd-manager.yaml
   ```

1. Get the token that was created for the ServiceAccount:

   ```bash
   kubectl get -n kube-system secret argocd-manager-token -o jsonpath='{.data.token}' | base64 --decode; echo
   ```

1. Make a copy of `example.yaml` at `./services/argo-cd/base/clusters` and rename it to `private.yaml`.

1. Update the cluster configuration file `private.yaml`:
   - Replace `<TOKEN>` with the result of step 2
   - Replace `<CERT_DATA>` with the certData in your `~/.kube/config`
   - Replace `<KEY_DATA>` with the keyData in your `~/.kube/config`
   - Replace `<API_SERVER_PORT>` with the port used to reach the API server locally.
     - Can be found in your `~/.kube/config`

1. Deploy argocd to your local cluster:

   ```bash
   kubectl apply -n argocd --server-side --force-conflicts -f ./services/argo-cd/base/clusters/private.yaml
   kubectl apply -n argocd --server-side --force-conflicts -k ./services/argo-cd
   ```

## Bootstrap

At first, the [sync-projects](./bootstrap/sync/sync-projects.yaml) application must be applied manually.

Then, the [bootstrap](./bootstrap/sync/bootstrap.yaml) can be applied manually.

```bash
kubectl apply -n argocd -f bootstrap/sync/sync-projects.yaml
kubectl apply -n argocd -f bootstrap/sync/bootstrap.yaml
```
