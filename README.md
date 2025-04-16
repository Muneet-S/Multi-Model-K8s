# Kubernetes Multi-Model Deployment
A production-ready Kubernetes deployment serving two isolated ML models with FastAPI.

## 📋 Features

- **Two independent models**:
  - Square Model (`x²` calculation)
  - Cube Model (`x³` calculation)
- **Kubernetes best practices**:
  - Separate deployments & services
  - Ingress routing
  - Load balancing
- **Developer-friendly**:
  - Clear documentation
  - One-command deployment
  - Test scripts included

## 🚀 Quick Start

### Prerequisites
- [Docker](https://docs.docker.com/get-docker/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

### Deployment
```bash
# Start cluster
minikube start --driver=docker
minikube addons enable ingress

# Build and deploy
kubectl apply -f k8s/

# Verify
kubectl get all

MINIKUBE_IP=$(minikube ip)

# Test square model
curl -X POST http://$MINIKUBE_IP/square/predict \
  -H "Content-Type: application/json" \
  -d '{"input": 5}'

# Test cube model
curl -X POST http://$MINIKUBE_IP/cube/predict \
  -H "Content-Type: application/json" \
  -d '{"input": 5}'

graph TD
    A[Client] --> B[Ingress]
    B --> C[Square Service]
    B --> D[Cube Service]
    C --> E[Square Pods]
    D --> F[Cube Pods]

.
├── models/
│   ├── square/               # Square model implementation
│   └── cube/                 # Cube model implementation
├── k8s/
│   ├── square-deployment.yaml
│   ├── cube-deployment.yaml
│   ├── services.yaml
│   └── ingress.yaml
├── README.md
└── .gitignore

# View logs
kubectl logs -l app=square-model

# Check resource usage
kubectl top pods
