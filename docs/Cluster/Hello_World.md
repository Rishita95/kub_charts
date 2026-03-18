# Hello World Deployment

This guide covers deploying a "Hello World" application on **every node** in the multi-node kind cluster using a Kubernetes **DaemonSet** with near-maximum CPU and RAM requests.

---

## Why a DaemonSet?

A **DaemonSet** ensures that a pod runs on **every node** in the cluster — including the control-plane. This is exactly what we need to run "Hello World" across all 3 nodes simultaneously.

---

## Node Resource Summary

Each node in the cluster has the following allocatable resources:

| Resource | Allocatable |
|----------|-------------|
| CPU | 5 cores |
| Memory | ~5 Gi (5133040 Ki) |

We request **3.5 CPU** and **3.5 Gi RAM** per pod — close to the maximum while leaving headroom for system components.

---

## 1. Create the DaemonSet Manifest

Create `hello-world-example/hello-world-ds.yaml` inside the `Cluster/` directory:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: hello-world
  namespace: default
spec:
  selector:
    matchLabels:
      name: hello-world
  template:
    metadata:
      labels:
        name: hello-world
    spec:
      tolerations:
      - key: "node-role.kubernetes.io/control-plane"
        operator: "Exists"
        effect: "NoSchedule"
      containers:
      - name: hello-world
        image: busybox:latest
        command:
        - /bin/sh
        - -c
        - "echo 'Hello World' && sleep infinity"
        resources:
          requests:
            cpu: "3500m"
            memory: "3500Mi"
          limits:
            cpu: "3500m"
            memory: "3500Mi"
```

> **Note:** The `tolerations` section is required to allow the pod to run on the control-plane node, which has a `NoSchedule` taint by default.

---

## 2. Deploy the DaemonSet

```bash
kubectl apply -f hello-world-example/hello-world-ds.yaml
```

Expected output:

```
daemonset.apps/hello-world created
```

---

## 3. Verify Pods are Running on All Nodes

```bash
kubectl get pods -l name=hello-world -o wide
```

Expected output:

```
NAME                READY   STATUS    RESTARTS   AGE   IP            NODE
hello-world-xxxxx   1/1     Running   0          ...   10.244.x.x    multinode-control-plane
hello-world-xxxxx   1/1     Running   0          ...   10.244.x.x    multinode-worker
hello-world-xxxxx   1/1     Running   0          ...   10.244.x.x    multinode-worker2
```

---

## 4. Check the Hello World Logs

To confirm the "Hello World" message was printed on any pod:

```bash
kubectl logs -l name=hello-world
```

Expected output:

```
Hello World
```

---

## 5. Delete the DaemonSet (Optional)

```bash
kubectl delete -f hello-world-example/hello-world-ds.yaml
```
