# Kind Cluster Setup

This guide walks through creating a multi-node Kubernetes cluster locally using **kind** with **Podman** as the container provider.

---

## 1. Create the Cluster Config

Create a file named `kind-config.yaml` inside the `Cluster/` directory:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

This config defines:
- **1 control-plane node** — manages the cluster
- **2 worker nodes** — run workloads

---

## 2. Start the Podman Machine

Before creating the cluster, make sure the Podman machine is running:

```bash
podman machine init   # only needed if not already initialized
podman machine start
```

---

## 3. Create the Cluster

Run the following command to create the cluster using Podman as the provider:

```bash
KIND_EXPERIMENTAL_PROVIDER=podman kind create cluster \
  --name multinode \
  --config kind-config.yaml
```

Expected output:

```
Creating cluster "multinode" ...
 ✓ Ensuring node image (kindest/node:v1.35.0) 🖼
 ✓ Preparing nodes 📦 📦 📦
 ✓ Writing configuration 📜
 ✓ Starting control-plane 🕹️
 ✓ Installing CNI 🔌
 ✓ Installing StorageClass 💾
 ✓ Joining worker nodes 🚜
Set kubectl context to "kind-multinode"
```

---

## 4. Verify the Cluster

> Switch context and check all nodes are Ready:

```bash
kubectl config use-context kind-multinode
kubectl get nodes
```

Expected output:

```
NAME                      STATUS   ROLES           AGE   VERSION
multinode-control-plane   Ready    control-plane   ...   v1.35.0
multinode-worker          Ready    <none>          ...   v1.35.0
multinode-worker2         Ready    <none>          ...   v1.35.0
```

---

## 5. Delete the Cluster (Optional)

To tear down the cluster when done:

```bash
KIND_EXPERIMENTAL_PROVIDER=podman kind delete cluster --name multinode
```
