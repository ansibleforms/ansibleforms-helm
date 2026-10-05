# AnsibleForms Helm charts

Helm charts for [AnsibleForms](https://ansibleforms.com), a web front end that turns Ansible
playbooks and AWX/AAP templates into self-service forms.

## Charts

| Chart | Description |
|---|---|
| [ansibleforms](charts/ansibleforms) | AnsibleForms and its MySQL database |

## Installing

From the chart repository:

```bash
helm repo add ansibleforms https://ansibleforms.github.io/helm-charts/
helm repo update
helm show values ansibleforms/ansibleforms > my_values.yaml
```

Or from the OCI registry: `oci://ghcr.io/ansibleforms/charts/ansibleforms`.

The [chart's README](charts/ansibleforms/README.md) covers every value, storage, ingress,
credentials and upgrades.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for the local checks, what CI runs and how a release
is cut. Changes are listed in the chart's [CHANGELOG.md](charts/ansibleforms/CHANGELOG.md).
