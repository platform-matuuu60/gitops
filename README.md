# IDP GitOps

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
apiVersion: platform.idp.internal/v1alpha1
kind: ServerlessApp
metadata:
  name: hello-api
  namespace: team-demo
spec:
  target: floci            # floci | aws
  artifact:
    bucket: idp-artifacts
    key: hello-api/<content-hash>.zip   # published by CI
```

The app's URL is reported in `status.url`:

```bash
kubectl -n team-demo get serverlessapp hello-api
```
