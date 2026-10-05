# Contributing

## Running the checks locally

Everything CI does can be run before pushing:

```bash
pip install yamllint
yamllint --strict .

cd charts/ansibleforms
helm lint . --strict
for f in ci/*-values.yaml ../../test/values/*-values.yaml; do
  helm lint . --strict --values "$f"
  helm template ansibleforms . --values "$f" | kubeconform -strict -summary
done
```

The chart is in `charts/ansibleforms`. Its `ci/*-values.yaml` are the scenarios `ct` installs on a kind cluster.
`test/values/*-values.yaml` are mostly render-only, for combinations that cannot
be installed unattended or that only exist to prove a manifest is shaped right.
Some of them are driven by hand from the workflow: the two `ingress-*` files are
installed behind a real controller, and `minimal-mysql` exists to prove the
chart still renders with its values cleared.

## Documenting a value

Every value carries a one-line `# --` comment directly above it, which
[helm-docs](https://github.com/norwoodj/helm-docs) turns into
`charts/ansibleforms/VALUES.md`. Section banners in values.yaml use `# ===`, because a line
starting with `# --` is read as a description. After changing a value or its comment,
regenerate the file:

```bash
helm-docs --chart-search-root charts --template-files VALUES.md.gotmpl \
  --output-file VALUES.md --ignore-non-descriptions
```

CI fails when VALUES.md does not match values.yaml.

## What CI checks

Four jobs, three of which need a cluster:

**Lint and render.** yamllint, `helm lint` on every values file, and `helm
template` plus `kubeconform -strict` across four Kubernetes versions. On top of
that a few assertions that rendering alone would not make:

- the chart refuses to write the placeholder credentials into a real Secret
- every Ingress backend resolves to a port its Service actually publishes,
  followed the way a controller would follow it
- every scheduling value comes out in the pod spec unchanged, `commonLabels` and
  `commonAnnotations` reach every object, and neither reaches a selector, which
  is immutable
- the MySQL probes exist and none of them carries a credential
- the chart version moved, if anything under `Chart.yaml`, `values.yaml`,
  `templates/` or `files/` (if it has one) of `charts/ansibleforms` did
- the two security context combinations that cannot work are still refused, and
  the message still names the value to change

`values.schema.json` is checked by Helm itself on every render and every
install, so a wrong type or a misspelled top level key fails before anything
reaches a cluster. It is deliberately strict at the top level and permissive
below it: a misspelling at the top is silent and sometimes dangerous, while a
stray key further down is usually someone's own leftover and should not block
their upgrade.

**Install on kind.** Runs three times, on Kubernetes 1.31, 1.33 and 1.35.
Rendering and installing catch different things: `kubeconform` reads a schema,
while an install is where a probe, a security context or a storage default meets
an actual kubelet. Each run does `ct install` for the `ci/` scenarios, which
also runs `helm test` after each one, plus the cases a values file alone cannot
describe: a Secret managed outside the chart, a database the chart does not
manage, two releases side by side in one namespace, and an install into a
namespace enforcing the restricted Pod Security Standard.

**Upgrade a live release.** Installs the newest published chart, writes a row
into the database and a file onto the server's volume, upgrades to the branch,
and checks that every claim still points at the same volume and that both
markers are still there. This is the one that catches a renamed
PersistentVolumeClaim, which every other check in the file would happily let
through: a renamed claim is an empty volume with the old one deleted underneath
it.

It then runs the same upgrade a second time and compares the pod names, because
a rollout checksum that moves when nothing changed would churn both pods on
every release. Finally it rolls the release back to the published version it
started from and checks the storage and the data again, walking the whole change
backwards.

**Enforce the NetworkPolicy.** kind ships kindnet, which does not enforce
NetworkPolicy, so this job turns the default CNI off and installs Calico. It
shows an unrelated pod reaching MySQL before the policy is on and not after,
checking the baseline first so a run where nothing could reach the database
anyway fails rather than quietly proving nothing.

**Route through a real ingress controller.** kind ships without one, so the
Ingress used to be rendered, validated and never asked for a single page. This
installs ingress-nginx and fetches the application through it, both plain and
with the application terminating TLS itself. That second case is where the port
bug fixed in 6.2.2 lived: a manifest that validates perfectly and that no
controller can route.

To try a real install:

```bash
helm upgrade --install test charts/ansibleforms \
  --namespace ansibleforms-test --create-namespace \
  --values charts/ansibleforms/ci/default-values.yaml --wait
```

## Cutting a release

Releases are driven by the `version` field in `charts/ansibleforms/Chart.yaml`. Bump it in a pull
request, merge to `main`, and the release workflow tags the version, attaches
the packaged chart to a GitHub release, pushes it to GHCR as an OCI artifact and
asks ansibleforms.com to rebuild, which serves the Helm repository at
`https://ansibleforms.com/helm-charts/`, indexed from these releases.

It runs after CI, not alongside it, and does nothing unless CI finished green,
so a failing build cannot publish. Merging with a version that already has a tag
is a no-op, which is what makes it safe to leave running on every push. To
rebuild the Helm repository index without releasing anything, run the workflow
by hand from the Actions tab.

`version` is the chart version and follows the chart's own changes. `appVersion`
tracks the AnsibleForms release the default image points at. They are no longer
kept in lockstep: a fix to a template does not need a new application release.

Anything that renames a resource, removes a value or forces an existing
Deployment to be recreated is a major bump, and belongs in `charts/ansibleforms/CHANGELOG.md` with
the steps an operator has to take.

CI refuses a pull request that changes `Chart.yaml`, `values.yaml`, `templates/`
or `files/` without moving the version, and refuses a version that has already
been released. Both would merge green and publish nothing, leaving the change on
`main` until some later release dragged it along. Changes that do not reach the
packaged chart, to CI, to `test/` or to the documentation, need no bump; the chart's
`ci/` is in its `.helmignore`, and `test/` sits outside the chart altogether.

## Following app releases

The chart's default image follows AnsibleForms by itself. An app release sends an
`app-release` dispatch here, and the *Follow app release* workflow opens a pull request on
the line of that major (`main`, or `release/<major>.x`) that moves the image, `appVersion`
and the Artifact Hub images annotation, bumps the chart's patch version and adds a
changelog entry. It merges itself once **CI passed** is green, and the release workflow
publishes the chart. A new major version opens an issue instead, because moving the chart
across a major takes real work. To catch up on a release by hand: Actions → *Follow app
release* → Run workflow, with the version.

## The 6.x maintenance line

`main` is the chart for AnsibleForms 7. Fixes for the 6 line (AnsibleForms 6.5) go to
`release/6.x`, which started from chart 6.3.7: branch from it, open the pull request
against it, and bump the version within 6.x (6.3.8, ...). The same rules as on `main`
apply: a pull request, squash merges, and Lint and render green.

The release workflow publishes both lines. A 6.x release goes to the same chart
repository and OCI registry, and is not marked as the latest GitHub release while a
higher version exists. CI's upgrade test starts from the newest published chart of the
branch's own major, so a 6.x pull request upgrades a 6.x release.

Users stay on 6 by pinning the chart: `--version "~6"`.

## Repository settings this depends on

The automatic `GITHUB_TOKEN` covers the GitHub release and the push to GHCR.

The website rebuild needs the `ansibleforms-release` GitHub App installed on this
repository, the App's ID as the `RELEASE_APP_ID` repository variable and its private key as
the `RELEASE_APP_PRIVATE_KEY` secret. Without them the release and the OCI artifact still go
out, the rebuild job says so in its run summary, and ansibleforms.com picks the release up
on its nightly build.

The GHCR package needs nothing. Pushed from Actions it is linked to the
repository and inherits its visibility, so on a public repository it can be
pulled anonymously right after the first release.
