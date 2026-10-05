<!--
The TITLE must be a Conventional Commit - `fix: ...`, `feat: ...` - because it becomes the
squash commit. See CONTRIBUTING.md.
-->

## What this changes

<!-- One or two sentences. If it follows an AnsibleForms change, link that pull request. -->

## Checklist

- [ ] Branch is named `<type>/<description>` and the title is a Conventional Commit
- [ ] Opened against the right line: `main` for the 7.x chart, `release/6.x` for the 6.x chart
- [ ] If anything under `charts/ansibleforms` changed: `version` in `Chart.yaml` moved and `CHANGELOG.md` has an entry
- [ ] If a value or its `# --` comment changed: `VALUES.md` is regenerated with helm-docs
