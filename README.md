# visualize-kubernetes-with-grafana

A deployment of Grafana within Kubernetes for its data visualization

## Prerequisits

1. Kubernetes cluster - If you don't have one available. You can use Minikube.
2. Kubectl
3. Helm - The package manager to install software on Kubernetes

### Run Minikube (Optional)

If you don't have a running Kubernetes cluster available, you can use minikube to spin up a local one. Follow the steps as described here: [Get started with Minikube](https://minikube.sigs.k8s.io/docs/start)

## Get Started

### Install Prometheus and Grafana using Helm

For the installation we can make use of the following Helm charts.

First, let's create a namespace where we are going to deploy the services

```bash
$ kubectl create namespace monitoring
namespace/monitoring created
```

Add the prometheus-community repository to helm.

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Install the prometheus-stack

```bash
helm install prometheus-stack prometheus-community/kube-prometheus-stack -n monitoring
```

Verify the deployment using

```bash
kubectl get pods -n monitoring
```

### Login to Prometheus and Grafana using Minikube

To be able to access the services, we need to forward their ports respectivley. Check the service endpoints using

```bash

```

For Prometheus

```bash

```

For Grafana

```bash

```
