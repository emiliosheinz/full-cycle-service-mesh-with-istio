# Full Cycle Service Mesh with Istio

This repository holds all the code and notes produced during the Service Mesh with Istio module of the Full Cycle course.

## 🛠️ Running Locally 

The following steps will guide you through the process of running the application locally. Make sure to check the prerequisites before starting.

1. Clone the repository

1. Install [Docker](https://docs.docker.com)

1. Install [k3d](https://k3d.io/)

1. Create the cluster with `k3d cluster create`

```bash
k3d cluster create -p "8000:30000@loadbalancer" --agents 2
```

1. Install [Istio](https://istio.io)

## 📚 Definitions

### Service Mesh

Service Mesh is an additional layer added to your cluster to monitor and modify application traffic in real-time, as well as enhance the security and reliability of the entire ecosystem.

### Istio

Istio is an open-source project that implements a service mesh aiming to reduce the complexity in managing distributed applications, regardless of the language or technology they were developed in. Despite being complex, even more complex are the problems it solves.

### K3d

K3d is a lightweight wrapper to run k3s (Rancher Lab’s minimal Kubernetes distribution) in docker. It makes it very easy to create single and multi-node k3s clusters in docker, e.g. for local development on Kubernetes.