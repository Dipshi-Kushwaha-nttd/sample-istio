# sample-istio
This repository contains the configuration for a Kubernetes-based microservice architecture using Argo CD for GitOps deployment and Istio for service mesh capabilities.

# Components
1. Istio Service Mesh
Istio provides traffic management, security, and observability features for our microservices.

Istio Ingress Gateway
Namespace: istio-system
Service: istio-ingressgateway
Ports:
80 → 8080 (HTTP)
443 → 8443 (HTTPS)
Function: Acts as the entry point for all external traffic, handles routing to internal services
Istio Sidecar Injector
The sidecar injector automatically injects Envoy proxy containers into application pods, which:

Intercepts all inbound and outbound traffic
Provides metrics, logs, and traces
Enforces security policies
Enables TLS encryption

3. Applications
Frontend Application
Namespace: yo-development
Service: frontend-app
Port: 80
Function: Serves the user interface
Backend Application
Namespace: yo-development
Service: backend-app
Port: 5000
Function: Provides API endpoints and business logic

5. Security Features in Istio
Mutual TLS (mTLS)
PeerAuthentication: Enforces STRICT mTLS between services
Benefits:
Encrypts all service-to-service communication
Provides strong identity for each service
Prevents unauthorized access
Authorization Policies
Default Policy: Deny-all
Custom Rules: Specific rules to allow necessary traffic
Function: Granularly controls which services can communicate with each other
Service Accounts and RBAC
Each component has its dedicated service account
Role-based access control limits what each service can do within the cluster
Prevents privilege escalation
Egress Traffic Control
Mode: REGISTRY_ONLY
Function: Only allows outbound traffic to registered external services, preventing data exfiltration
Installation Guide
Prerequisites
Kubernetes cluster up and running
kubectl CLI tool installed
Homebrew installed (for macOS users)
Git CLI tool installed

1. Installing Istio
Install the Istio CLI and deploy Istio components:

# Install istioctl using Homebrew
brew install istioctl

# Install Istio with the demo profile
istioctl install --set profile=demo -y

# Enable Istio sidecar injection in the default namespace
kubectl label namespace default istio-injection=enabled
3. Installing Monitoring Tools
Deploy Kiali dashboard for service mesh visualization:

kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.26/samples/addons/kiali.yaml
Deploy Grafana for metrics visualization:

kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.20/samples/addons/grafana.yaml
Deploy Prometheus for metrics collection:

kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.20/samples/addons/prometheus.yaml
Access the dashboards:

# Kiali dashboard
kubectl port-forward svc/kiali -n istio-system 20001:20001

# Grafana dashboard
kubectl port-forward svc/grafana -n istio-system 3000:3000
Access Kiali at http://localhost:20001 and Grafana at http://localhost:3000.
