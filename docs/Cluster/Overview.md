# Cluster Overview

This section covers the setup and management of a local Kubernetes cluster using **kind** (Kubernetes IN Docker) with **Podman** as the container provider on macOS.

## What is kind?

[kind](https://kind.sigs.k8s.io/) is a tool for running local Kubernetes clusters using container nodes. It was primarily designed for testing Kubernetes itself, but it is also useful for:

- Local development and testing
- CI pipelines
- Multi-node cluster simulation on a single machine

## Why Podman?

[Podman](https://podman.io/) is a daemonless, rootless container engine that is a drop-in replacement for Docker. On macOS, kind supports Podman as an experimental provider via:

```bash
KIND_EXPERIMENTAL_PROVIDER=podman kind ...
```

## Cluster Structure

The cluster in this project is a **multi-node** cluster with:

| Node | Role |
|------|------|
| `multinode-control-plane` | Control Plane (manages the cluster) |
| `multinode-worker` | Worker Node 1 |
| `multinode-worker2` | Worker Node 2 |

## Directory Structure

```
kub_charts/
└── Cluster/
    ├── assets
    ├── kind-config.yaml
    └── hello-world-example/
        └── hello-world-ds.yaml
```
