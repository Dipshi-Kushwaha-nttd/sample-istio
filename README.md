# sample-istio
This repository contains the configuration for a Kubernetes-based microservice architecture using Istio for service mesh capabilities.

# Components
### 1. Istio Service Mesh

Istio provides traffic management, security, and observability features for our microservices.

#### Istio Ingress Gateway

- **Namespace**: `istio-system`
- **Service**: `istio-ingressgateway`
- **Ports**: 
  - 80 → 8080 (HTTP)
  - 443 → 8443 (HTTPS)
- **Function**: Acts as the entry point for all external traffic, handles routing to internal services

#### Istio Sidecar Injector

The sidecar injector automatically injects Envoy proxy containers into application pods, which:
- Intercepts all inbound and outbound traffic
- Provides metrics, logs, and traces
- Enforces security policies
- Enables TLS encryption

### Security Features in Istio
#### Mutual TLS (mTLS)

- **PeerAuthentication**: Enforces STRICT mTLS between services
- **Benefits**:
  - Encrypts all service-to-service communication
  - Provides strong identity for each service
  - Prevents unauthorized access

#### Authorization Policies

- **Default Policy**: Deny-all
- **Custom Rules**: Specific rules to allow necessary traffic
- **Function**: Granularly controls which services can communicate with each other

#### Service Accounts and RBAC

- Each component has its dedicated service account
- Role-based access control limits what each service can do within the cluster
- Prevents privilege escalation

#### Egress Traffic Control

- **Mode**: REGISTRY_ONLY
- **Function**: Only allows outbound traffic to registered external services, preventing data exfiltration

## Installation Guide

### Prerequisites

- Kubernetes cluster up and running
- `kubectl` CLI tool installed
- Git CLI tool installed

# Installing Istio
Install the Istio CLI and deploy Istio components:

```bash
# Install istioctl in Windows
wget https://github.com/istio/istio/releases/download/1.27.0/istio-1.27.0-linux-amd64.tar.gz
tar-xvf istio-1.27.0-linux-amd64.tar.gz
cd istio-1.27.0/
export PATH="$PWD/bin:$PATH"
istioctl version </pre>

# Install Istio with the demo profile
istioctl install --set profile=demo -y

# Enable Istio sidecar injection in the default namespace
kubectl label namespace default istio-injection=enabled

```

## 2. Installing Monitoring Tools

### Deploy Kiali dashboard for service mesh visualization:

```bash
kubectl apply -f samples/addons <br>
kubectl rollout status deployment/kiali -n istio-system
```

### Access the dashboards:

```bash
# Kiali dashboard
kubectl port-forward svc/kiali -n istio-system 20001:20001

# Grafana dashboard
kubectl port-forward svc/grafana -n istio-system 3000:3000
```

#### Access Kiali at http://localhost:20001 and Grafana at http://localhost:3000.
