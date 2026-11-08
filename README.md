# k8s-apps-infrastructure

## Team-1 Flux prerequisites

Create these secrets in namespace `team-1` with an administrator-approved
credential. Do not commit the generated Secret manifests.

```sh
kubectl -n team-1 create secret docker-registry ghcr-read \
  --docker-server=ghcr.io \
  --docker-username="$GHCR_USERNAME" \
  --docker-password="$GHCR_TOKEN"

kubectl -n team-1 create secret generic apps-infra-git-write \
  --from-file=identity="$FLUX_GIT_WRITE_KEY" \
  --from-file=known_hosts="$FLUX_GIT_KNOWN_HOSTS"
```

The `apps-infra-git-write` key must be restricted to this repository and the
`main` branch policy must be approved by the repository administrator.

The Helm chart source is `chart/`; publish it to:
`oci://ghcr.io/CyberBlock-SPDA/k8s-apps-infrastructure`.

```sh
helm package chart
helm push k8s-apps-infrastructure-0.1.0.tgz \
  oci://ghcr.io/CyberBlock-SPDA
```

Verification:

```sh
flux get sources oci -n team-1
flux get helmreleases -n team-1
flux get image repository -n team-1
flux get image policy -n team-1
flux get image update -n team-1
```
