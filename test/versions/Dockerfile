# The versions CI tests against, one FROM line each, kept here so Dependabot can bump them:
# its docker ecosystem reads Dockerfiles, not workflow variables. Never built; the
# workflows read the image references from this file.
#
#   kind-node    The Kubernetes every kind cluster runs. Its tag is the Kubernetes version,
#                which the kubeconform schema check uses too. A node image needs the kind
#                release that built it; kind comes from helm/kind-action, whose default
#                follows kind, and Dependabot bumps the action.
#   kubeconform  The schema validator.
#   calico       The Calico release whose manifest the NetworkPolicy job installs.

FROM kindest/node:v1.37.0@sha256:a1ed56cfb0e7b93589bdf97c8cd566405a265939e3620fc4f5de89adff580ae5 AS kind-node
FROM ghcr.io/yannh/kubeconform:v0.8.0 AS kubeconform
FROM calico/node:v3.33.0 AS calico
