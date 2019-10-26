# Kustomize Plugins

Plugins for kustomize that we've found useful at Discogs!

## Installation

```sh
cp -pr platform.discogs.com ~/.config/kustomize/plugins
```

## Plugins

### HelmChartGenerator

Generate resources from a Helm chart. Supports basic substitution in values.

```yaml
apiVersion: platform.discogs.com/v1
kind: HelmChartGenerator
metadata:
  name: release-name
  namespace: release-namespace
chart: stable/some-chart
version: 1.0.0
values:
  someValue: foo
  anotherValue:
  - baz
  - bat
  substitutionExamples:
    fromFile: $(file://someFile)
    fromFileIncludingNewline: $(file://someFile?trim=false)
    fromEnvFile: $(envfile://secrets.env#ENV_VAR_NAME)
    fromEnvVar: $(env://ENV_VAR_NAME)
    fromVault: $(vault://path/to/secret#field)
    withSurroundingContent: foo-$(env://ENV_VAR_NAME)-bar
```

### HTTPGenerator

Generate resources from URLs.

```yaml
apiVersion: platform.discogs.com/v1
kind: HTTPGenerator
metadata:
  name: calico
resources:
- url: https://docs.projectcalico.org/v3.10/manifests/calico.yaml
  integrity: sha384-rPa2FbjIL5YlY7sCvycTmDgCcz9YZjHV40tGtn+azvcAKH5IxIcxxDvpN0B1Pc1W
```

### VaultSecretGenerator

Generate Secrets by reading data from Vault. Supports basic substitution.

```yaml
apiVersion: platform.discogs.com/v1
kind: VaultSecretGenerator
metadata:
  name: my-secret
data:
  someSecret: $(path/to/secret#field)
  withSurroundingContent: foo-$(some/other/secret#token)-bar
disableNameSuffixHash: false  # optional, default false
```
