# sample-istio
This repository contains the configuration for a Kubernetes-based microservice architecture using Istio for service mesh capabilities.

# Components
## 1. Istio Service Mesh
Istio provides traffic management, security, and observability features for our microservices.

### Istio Ingress Gateway
**Namespace:** istio-system <br> **Service:** istio-ingressgateway <br> **Ports:**
80 → 8080 (HTTP)
443 → 8443 (HTTPS) <br> **Function:** Acts as the entry point for all external traffic, handles routing to internal services

### Istio Sidecar Injector
The sidecar injector automatically injects Envoy proxy containers into application pods, which:<br>

Intercepts all inbound and outbound traffic <br> Provides metrics, logs, and traces <br> Enforces security policies <br> Enables TLS encryption

### Security Features in Istio
#### Mutual TLS (mTLS)
**PeerAuthentication:** Enforces STRICT mTLS between services
#### Benefits:
Encrypts all service-to-service communication <br> Provides strong identity for each service <br> Prevents unauthorized access 
#### Authorization Policies
**Default Policy:** Deny-all <br> **Custom Rules:** Specific rules to allow necessary traffic<br> **Function:** Granularly controls which services can communicate with each other 
**Service Accounts and RBAC** <br>
Each component has its dedicated service account <br> Role-based access control limits what each service can do within the cluster<br>  Prevents privilege escalation
#### Egress Traffic Control
**Mode:** REGISTRY_ONLY  **Function:** Only allows outbound traffic to registered external services, preventing data exfiltration

## Installation Guide
#### Prerequisites
Kubernetes cluster up and running  kubectl CLI tool installed  Git CLI tool installed

# Installing Istio
Install the Istio CLI and deploy Istio components:

### Install istioctl in Windows
` wget https://github.com/istio/istio/releases/download/1.27.0/istio-1.27.0-linux-amd64.tar.gz <br> tar -xvf istio-1.27.0-linux-amd64.tar.gz <br> cd istio-1.27.0/ <br> export PATH="$PWD/bin:$PATH" <br> istioctl version `

### Install Istio with the demo profile
`istioctl install --set profile=demo -y`

### Enable Istio sidecar injection in the default namespace
`kubectl label namespace default istio-injection=enabled`

## 2. Installing Monitoring Tools

## Deploy Kiali dashboard for service mesh visualization:

`kubectl apply -f samples/addons <br>
kubectl rollout status deployment/kiali -n istio-system`


## Access the dashboards:

### Kiali dashboard
`kubectl port-forward svc/kiali -n istio-system 20001:20001`

### Grafana dashboard
`kubectl port-forward svc/grafana -n istio-system 3000:3000`

#### Access Kiali at http://localhost:20001 and Grafana at http://localhost:3000.
