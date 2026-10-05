# Changelog

## 6.5.3

AnsibleForms 6.5.3.

### Changed

- **The default image is AnsibleForms 6.5.3**, `ghcr.io/ansibleforms/ansibleforms:6.5.3`,
  and `appVersion` follows. See the
  [6.5.3 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.5.3).

## 6.5.2

AnsibleForms 6.5.2.

### Changed

- **The default image is AnsibleForms 6.5.2**, `ghcr.io/ansibleforms/ansibleforms:6.5.2`,
  and `appVersion` follows. See the
  [6.5.2 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.5.2).

## 6.5.1

AnsibleForms 6.5.1.

### Changed

- **The default image is AnsibleForms 6.5.1**, `ghcr.io/ansibleforms/ansibleforms:6.5.1`,
  and `appVersion` follows. See the
  [6.5.1 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.5.1).

## 6.5.0

AnsibleForms 6.5.0.

### Changed

- **The default image is AnsibleForms 6.5.0**, `ghcr.io/ansibleforms/ansibleforms:6.5.0`,
  and `appVersion` follows. See the
  [6.5.0 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.5.0).

## 6.4.1

AnsibleForms 6.4.1.

### Changed

- **The default image is AnsibleForms 6.4.1**, `ghcr.io/ansibleforms/ansibleforms:6.4.1`,
  and `appVersion` follows. See the
  [6.4.1 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.4.1).

## 6.4.0

AnsibleForms 6.4.0.

### Changed

- **The default image is AnsibleForms 6.4.0**, `ghcr.io/ansibleforms/ansibleforms:6.4.0`,
  and `appVersion` follows. See the
  [6.4.0 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.4.0).

## 6.3.1

AnsibleForms 6.3.1.

### Changed

- **The default image is AnsibleForms 6.3.1**, `ghcr.io/ansibleforms/ansibleforms:6.3.1`,
  and `appVersion` follows. See the
  [6.3.1 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.3.1).

## 6.3.0

AnsibleForms 6.3.0.

### Changed

- **The default image is AnsibleForms 6.3.0**, `ghcr.io/ansibleforms/ansibleforms:6.3.0`,
  and `appVersion` follows. See the
  [6.3.0 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.3.0).

## 6.2.1

AnsibleForms 6.2.1.

### Changed

- **The default image is AnsibleForms 6.2.1**, `ghcr.io/ansibleforms/ansibleforms:6.2.1`,
  and `appVersion` follows. See the
  [6.2.1 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.2.1).

## 6.2.0

AnsibleForms 6.2.0.

### Changed

- **The default image is AnsibleForms 6.2.0**, `ghcr.io/ansibleforms/ansibleforms:6.2.0`,
  and `appVersion` follows. See the
  [6.2.0 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.2.0).

## 6.1.5

AnsibleForms 6.1.5.

### Changed

- **The default image is AnsibleForms 6.1.5**, `ghcr.io/ansibleforms/ansibleforms:6.1.5`,
  and `appVersion` follows. See the
  [6.1.5 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.1.5).

## 6.1.4

AnsibleForms 6.1.4.

### Changed

- **The default image is AnsibleForms 6.1.4**, `ghcr.io/ansibleforms/ansibleforms:6.1.4`,
  and `appVersion` follows. See the
  [6.1.4 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.1.4).

## 6.1.2

AnsibleForms 6.1.2.

### Changed

- **The default image is AnsibleForms 6.1.2**, `ghcr.io/ansibleforms/ansibleforms:6.1.2`,
  and `appVersion` follows. See the
  [6.1.2 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.1.2).

## 6.1.0

AnsibleForms 6.1.0.

### Changed

- **The default image is AnsibleForms 6.1.0**, `ghcr.io/ansibleforms/ansibleforms:6.1.0`,
  and `appVersion` follows. See the
  [6.1.0 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.1.0).

## 6.0.2

AnsibleForms 6.0.2.

### Changed

- **The default image is AnsibleForms 6.0.2**, `ghcr.io/ansibleforms/ansibleforms:6.0.2`,
  and `appVersion` follows. See the
  [6.0.2 release notes](https://github.com/ansibleforms/ansibleforms/releases/tag/6.0.2).

## 6.0.0

AnsibleForms 6.0.0, and the first release of the chart. From here on the chart version is
the AnsibleForms version it installs.

### Added

- **The server and a bundled MySQL**, each with its own PersistentVolumeClaim, a Service, and
  an optional Ingress with HTTP or HTTPS. `files/schema.sql` creates the base schema on the
  first start.
- **Credentials from a Secret**: the chart's own `<release>-secrets`, one you manage
  (`secrets.existingSecret`), or values the chart generates and keeps across upgrades
  (`secrets.generate`). It refuses to render the placeholder passwords from `values.yaml`.
- **A database of your own** with `mysql.enabled: false` and `applications.mysql.host`.
- **The restricted Pod Security Standard**: both pods run as a pinned non-root user with
  `fsGroup` owning their volumes, and no root init container.
- **Health checks** on both components, with a startup probe that holds the liveness probe
  back while the database initialises.
- **`networkPolicy.enabled`**: only the server may open a connection to the bundled MySQL.
- **Rollout checksums**: changing the Secret or `mysql.config` restarts what reads it, and
  `rollOnChange.external` does the same for objects the chart does not own.
- **Scheduling and metadata** on both components: `nodeSelector`, `tolerations`, `affinity`,
  `topologySpreadConstraints`, `imagePullSecrets`, `commonLabels`, `commonAnnotations`,
  `podLabels`, `podAnnotations`, `extraEnv`, `extraEnvFrom`, `extraVolumes` and service
  annotations.
- **`values.schema.json`**, a `NOTES.txt` and a `helm test` that fetches the front page and
  checks the database answers.
- **Forms and `custom.js` from ConfigMaps** (`forms.configMap`, `forms.extraFormsConfigMap`, `forms.customJs`).

### Upgrading from the chart in the repository before its first release

The chart was installed from a clone before it was released. Coming from there:

- **Four resources are renamed**: `server`, `mysql`, `mysql-my-cnf` and `mysql-init-script`
  become `<release>-server`, `<release>-mysql`, `<release>-mysql-my-cnf` and
  `<release>-mysql-init`. Helm creates the new ones and removes the old, which rolls both
  pods once. The Secret, the Ingress and both PersistentVolumeClaims keep their names, so
  no storage is touched.
- **`applications.mysql.host` defaults to `<release>-mysql`** instead of the literal `mysql`.
- **Selectors change**: anything selecting on `app.kubernetes.io/name: server` or
  `name: mysql` needs `app.kubernetes.io/component: server` or `component: database`.
- **`mysql_deployment.enabled` is now `mysql.enabled`.**
- **The `prepare-persistent-volume` init container is gone**; delete it from a copy of
  `values.yaml` you keep. If your storage ignores `fsGroup` (NFS with `root_squash`), set
  `containers.<component>.podSecurityContext` and `securityContext` to `null` and put it back.
- **The server's update strategy is `Recreate`**, since the volume is ReadWriteOnce.
