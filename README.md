# visualize-kubernetes-cluster-via-grafana

This project demonstrates how to deploy Prometheus and Grafana on a Minikube cluster to visualize the metrics from the Kubernetes cluster. Prometheus is used for collecting and storing metrics, while Grafana is used for visualizing these metrics in an easy-to-understand dashboard.

## Table of Contents

- [Purpose](#purpose)
- [Prerequisits](#prerequisits)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Contributing](#contributing)

## Purpose

### Prometheues

Prometheus is an open-source monitoring system and time-series database. In this context, it is used to collect metrics from the Kubernetes cluster and its applications. Prometheus scrapes metrics from configured endpoints at specified intervals, evaluates rule expressions, displays results, and triggers alerts if certain conditions are observed.

### Grafana

Grafana is an open-source platform for monitoring and observability. It allows you to query, visualize, alert on, and understand your metrics no matter where they are stored. In this context, Grafana is used to create interactive and dynamic dashboards to visualize the metrics collected by Prometheus from the Kubernetes cluster.

## Prerequisits

Before you begin, ensure you have the following installed:

- [Minikube](https://minikube.sigs.k8s.io/docs/start)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Helm](https://helm.sh/docs/intro/install/)

## Installation

### 1. Start Minikube

Start your Minikube cluster with sufficient resources:

```bash
minikube start --memory=4096 --cpus=2
```

### 2. Deploy Prometheus

Add the Prometheus Helm repository:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Create a namespace in which to install Prometheus

```bash
kubectl create namespace monitoring
```

Install Prometheus using Helm:

```bash
helm install prometheus prometheus-community/prometheus --namespace monitoring
```

### 3. Deploy Grafana

Add the Grafana Helm repository:

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

Install Grafana using Helm into the same namespace as Prometheus:

```bash
helm install grafana grafana/grafana --namespace monitoring
```

### 4. Access Grafana Dashboard

Let's verify first that the deployment was successful:

Verify the deployment using

```bash
kubectl get pods -n monitoring
```

![Verify that the pods are running](images/pods.png)

Forward the Grafana service port to your local machine:

```bash
kubectl port-forward service/grafana 3000:80 --namespace monitoring
```

Get the admin password

```bash
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Now, open your browser and go to [http://localhost:3000](http://localhost:3000). Sign in by using the username *admin* and the password that you retrieved by running the previous command.

## Configuration

Get the service endpoint of the Prometheus server:

```bash
kubectl get svc --namespace monitoring | grep prometheus-server
```

![endpoint prometheus-server](images/prometheus-server-endpoint.png)

After logging into Grafana, add Prometheus as a data source:

1. Go to Connections > Add new connection.
2. Select "Prometheus".
3. Set the URL to match the prometheus-server endpoint, e.g. *http://10.101.162.137:80*.
4. Click "Save & Test".

Create dashboards in Grafana to visualize metrics collected by Prometheus.

![View metrics](images/metrics.png)

## Usage

- Monitor cluster health and performance metrics.
- Visualize metrics using pre-built or custom Grafana dashboards.
- Set up alerts in Grafana based on Prometheus metrics.

## Contributing

Contributions are welcome! Please submit a pull request or open an issue to discuss changes.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
