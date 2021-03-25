# Deploy KinD cluster

1. Create a KinD cluster.

```bash
kind create cluster --config=kind.config.yaml
```


2. Add nginx ingress controller to cluster. Ingress must be enabled in cluster.

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml

# Wait until it's created
kubectl wait --namespace ingress-nginx `
  --for=condition=ready pod `
  --selector=app.kubernetes.io/component=controller `
  --timeout=90s
```


3. Load docker image to KinD cluster.

```bash
# Load image to nodes
kind load docker-image {image-name}

# Open bash inside control-plane
docker exec -it kind-control-plane bash

# List containerd images inside cluster nodes
crictl images
```


4. Deploy k8s definitions into cluster.

```bash
# Get current cluster (kubectl config --help)
kubectl config current-context

# Set default namespace
kubectl config set-context --current --namespace=demo-ns

# Apply changes to cluster
kubectl apply -f contin-spa.yaml
```