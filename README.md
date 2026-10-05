# AnsibleForms Helm charts

[![CI](https://img.shields.io/github/actions/workflow/status/ansibleforms/helm-charts/ci.yaml?branch=main&label=CI)](https://github.com/ansibleforms/helm-charts/actions/workflows/ci.yaml)
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/ansibleforms)](https://artifacthub.io/packages/search?repo=ansibleforms)
[![License](https://img.shields.io/badge/license-GPL--3.0-blue)](LICENSE)
[![Docs](https://img.shields.io/badge/docs-ansibleforms.com-informational)](https://ansibleforms.com)

The Helm chart that runs [AnsibleForms](https://github.com/ansibleforms/ansibleforms) and its MySQL database on
Kubernetes, tested on every change by installing and upgrading it on three Kubernetes versions.
The installation guide and the rest of the documentation are at [ansibleforms.com](https://ansibleforms.com/installation).

## Charts

| Chart | Description |
|---|---|
| [ansibleforms](charts/ansibleforms) | AnsibleForms and its MySQL database |

## Installing

The chart is published to a Helm repository and to the GitHub Container Registry as an OCI artifact.
Add the repository and write the default values to a file you can edit:

```bash
helm repo add ansibleforms https://ansibleforms.com/helm-charts/
helm repo update
helm show values ansibleforms/ansibleforms > my_values.yaml
```

Or install from the OCI registry: `oci://ghcr.io/ansibleforms/charts/ansibleforms`. The [chart's README](charts/ansibleforms/README.md)
covers storage, ingress, credentials and upgrades, and [VALUES.md](charts/ansibleforms/VALUES.md) lists every value with its default.

## Versions

Each branch releases the charts for one AnsibleForms major, with its own chart versions and changelog.

| Branch | Default image |
|---|---|
| `main` | AnsibleForms 7 |
| `release/6.x` | AnsibleForms 6 |

## Contributing

Contributions are welcome. Start with these files:

- [CONTRIBUTING.md](CONTRIBUTING.md): the local checks, what CI runs and how a release is cut
- [CHANGELOG.md](charts/ansibleforms/CHANGELOG.md): the changes in every chart release
- [SECURITY.md](SECURITY.md): how to report a security issue

## License

[GPL-3.0](LICENSE).
