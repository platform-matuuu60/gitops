# ShopCo GitOps

Desired state of every team's workloads. ArgoCD syncs this repository into the cluster; the platform APIs (defined in [platform](https://github.com/platform-matuuu60/platform)) turn each request into real infrastructure.

Changes arrive as pull requests, opened by developers or by Backstage templates. Rolling back is `git revert`.

## Layout

```
teams/
  <team>/
    namespace.yaml     # one namespace per team
    <app>.yaml         # platform API requests (ServerlessApp, ...)
```

## Example

```yaml
apiVersion: platform.shopco.internal/v1alpha1
kind: ServerlessApp
metadata:
  name: hello-api
  namespace: team-shop
spec:
  target: floci            # floci | aws
  artifact:
    bucket: shopco-artifacts
    key: hello-api/<content-hash>.zip   # published by CI
```

The app's URL is reported in `status.url`:

```bash
kubectl -n team-shop get serverlessapp hello-api
```
