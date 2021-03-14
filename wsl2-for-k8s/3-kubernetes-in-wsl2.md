# Kubernetes in WSL2

## Install KinD in WSL2
Go to WSL2 distro and install KinD from official GitHub repo.
```bash
# Download the latest version of KinD
curl -Lo ./kind https://github.com/kubernetes-sigs/kind/releases/download/v0.7.0/kind-linux-amd64
# Make the binary executable
chmod +x ./kind
# Move the binary to your executable path
sudo mv ./kind /usr/local/bin/
```

## Create your first cluster in KinD
```bash
# Check if the KUBECONFIG is not set
echo $KUBECONFIG
# Check if the .kube directory is created > if not, no need to create it
ls $HOME/.kube
# Create the cluster and give it a name (optional)
kind create cluster --name wslkind
# Check if the .kube has been created and populated with files
ls $HOME/.kube
```
k8s master status can be check in masters url.

## Create multi-node cluster in KinD
```bash
# Delete the existing cluster
kind delete cluster --name wslkind
# Create a config file for a 3 nodes cluster
cat << EOF > kind-3nodes.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
  - role: worker
EOF
# Create a new cluster with the config file
kind create cluster --name wslkindmultinodes --config ./kind-3nodes.yaml
# Check how many nodes it created
kubectl get nodes
```
Check cluster services:
```bash
# Check the services for the whole cluster
kubectl get all --all-namespaces
```

## Configure kubernetes dashboard
Install kubernetes dashboard
```bash
# Install the Dashboard application into our cluster
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-rc6/aio/deploy/recommended.yaml
# Check the resources it created based on the new namespace created
kubectl get all -n kubernetes-dashboard
```

Create a proxy to expose dashboard
```bash
# Start a kubectl proxy
kubectl proxy
# Enter the URL on your browser: http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

Enter kubeconfig file of our cluster (it resided in .kube/config). RBAC approach should be follow to avoid errors.
```bash
# Create a new ServiceAccount
kubectl apply -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
EOF
# Create a ClusterRoleBinding for the ServiceAccount
kubectl apply -f - <<EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```
Create a token and sign in into dashboard
```bash
# Get the Token for the ServiceAccount
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
# Copy the token and copy it into the Dashboard login and press "Sign in"
```