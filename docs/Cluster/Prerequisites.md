# Prerequisites

Before setting up the multi-node kind cluster, ensure the following tools are installed on your macOS machine.

## 1. Homebrew

Homebrew is the package manager used to install all other tools.

> Check if Homebrew is installed:

```bash
brew --version
```

> If not installed, install it:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

---

## 2. Podman

Podman is used as the container provider for kind on macOS.

> Install Podman:

```bash
brew install podman
```

> Initialize and start the Podman machine:

```bash
podman machine init
podman machine start
```

> Verify:

```bash
podman --version
```

---

## 3. kind

kind (Kubernetes IN Docker) is used to create local Kubernetes clusters.

> Install kind:

```bash
brew install kind
```

> Verify:

```bash
kind version
```

---

## 4. kubectl

`kubectl` is the Kubernetes command-line tool to interact with your cluster.

> Install kubectl:

```bash
brew install kubectl
```

> Verify:

```bash
kubectl version --client
```

---

## 5. GitHub CLI (gh)

The GitHub CLI is used for repository management and authentication.

> Install gh:

```bash
brew install gh
```

> Authenticate:

```bash
gh auth login
```
