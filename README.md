# PyCamp-rps

## Description

This is a simple manifest that enables the execution of the images built for the [Kaggle rock, paper, scissors](https://www.kaggle.com/competitions/rock-paper-scissors) competition. Its main points are:

- Agent loader deployment:
  - **fettleif/pycamp-rps-load-agents:0.0.1** image
  - **AGENT_DIR** environment variable
  - **shared-agents** volume
- Agent runner deployment:
  - **fettleif/pycamp-rps-tournament-runner:0.0.2** image
  - **COMPETE_AGENT_DIR** environment variable from ConfigMap
  - **AGENT_DIR** environment variable from ConfigMap
  - **shared-agents** volume
  - **dummy** volume
- Persistent Volume Claim:
  - Acts as a shared volume for the loader and the runner
- Config Map
  - **COMPETE_AGENT_DIR_CM** value definition
  - **AGENT_DIR_CM** value definition
- Loader Service
  - Enables communication to the _agent-loader_ deployment
- Runner Service
  - Enables communication to the _agent-runner_ deployment

## Prerequisites

- [Install Docker](https://docs.docker.com/get-docker/)

- [Install Kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)

- [Install Kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)

## How to create the cluster

Once _kind_ is installed, it is as easy as running:

```bash
kind create cluster
```

[Documentation](https://kind.sigs.k8s.io/docs/user/quick-start/#creating-a-cluster)

## How to apply the manifest

While positioned at root folder, run:

```bash
kubectl apply -f app.yaml
```

[Documentation](https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-apply)

# How to access the services

After applying the manifets, it will be possible to perform a _port-forward_ to redirect traffic to a port on the local machine to a port on the selected service within the cluster.

For example, to access the **Loader Service**:

```bash
kubectl port-forward services/agent-loader-service 8000:80
```

This will allow access on `localhost:8000` and _forward_ the traffic to the _agent-loader-service_ on port 80.

Likewise, this would do the same but for the **Runner Service**:

```bash
kubectl port-forward services/agent-runner-service 8080:8080
```

[Documentation](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/#forward-a-local-port-to-a-port-on-the-pod)
