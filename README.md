# Full Cycle Service Mesh with Istio

This repository holds all the code and notes produced during the Service Mesh with Istio module of the Full Cycle course.

## 🛠️ Running Locally 

The following steps will guide you through the process of running the application locally. Make sure to check the prerequisites before starting.

1. Clone the repository

1. Install [Docker](https://docs.docker.com)

1. Install [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

1. Install [k3d](https://k3d.io/)

1. Create the cluster with `k3d cluster create`

    ```bash
    k3d cluster create -p "8000:30000@loadbalancer" --agents 2
    ```

1. Install [istioctl](https://istio.io)

1. Install Istio into the cluster

    ```bash
    istioctl install -y
    ```

1. Label the default namespace to enable automatic sidecar injection

    ```bash
    kubectl label namespace default istio-injection=enabled
    ```

1. Apply Kubertenes resources

    ```bash
    kubectl apply -f .
    ```

1. Add any addons you want to the cluster. Example:

    ```bash
    kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.20/samples/addons/prometheus.yaml && \
    kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.20/samples/addons/grafana.yaml && \
    kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.20/samples/addons/kiali.yaml    
    ```

1. Create Fortio deployment

    ```bash
    kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.20/samples/httpbin/sample-client/fortio-deploy.yaml
    ```


## 🔥 Sending Traffic to the Cluster

In order to make it easier to send traffic to the cluster, we are gonna use Fortio. Fortio is a load testing tool that can be used to send traffic to a specific endpoint.

1. Export the Fortio pod into a variable

    ```bash
    export FORTIO_POD=$(kubectl get pod -lapp=fortio -o 'jsonpath={.items[0].metadata.name}')
    ```

1. Send traffic to the cluster

    ```bash
    kubectl exec "$FORTIO_POD" -c fortio -- fortio load -c 2 -qps 0 -t 200s -loglevel Warning http://nginx-service:8000
    ```

## 📊 Monitoring

To access Kiali's dashboard, run the following command:

```bash
istioctl dashboard kiali
```

## 📚 Definitions

### Service Mesh

Service Mesh is an additional layer added to your cluster to monitor and modify application traffic in real-time, as well as enhance the security and reliability of the entire ecosystem.

### Istio

Istio is an open-source project that implements a service mesh aiming to reduce the complexity in managing distributed applications, regardless of the language or technology they were developed in. Despite being complex, even more complex are the problems it solves.

### K3d

K3d is a lightweight wrapper to run k3s (Rancher Lab’s minimal Kubernetes distribution) in docker. It makes it very easy to create single and multi-node k3s clusters in docker, e.g. for local development on Kubernetes.

### Gateway

Manages incoming and outgoing traffic. It works on layers 4-6, ensuring port, host, and TLS management. It is directly connected to a Virtual Service that will be responsible for routing.

### Virtual Service

A Virtual Service allows you to configure how requests will be routed to a service. It has a series of rules that, when applied, will ensure that the request is directed to the correct destination.

### Destination Rules

Destination Rules are used to configure the behavior of the traffic that reaches a service. It is possible to configure the load balancing algorithm, timeouts, and circuit breakers.