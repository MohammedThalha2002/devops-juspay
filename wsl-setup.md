# SETUP IN WSL

- UPDATE PACKAGES

```
sudo apt-get update
```

- Install Docker using this command

```bash
sudo apt install docker.io
```

- Install GO Language

```bash

wget https://go.dev/dl/go1.20.5.linux-amd64.tar.gz
sudo tar -xvf go1.20.5.linux-amd64.tar.gz
sudo mv go /usr/local

# open .bashrc file
vim .bashrc
# add these lines at the end of the file
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH
#
source .bashrc
#check
go version
```

- Install Kind

```bash

[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

- Install Kubectl

```bash
sudo snap install kubectl --classic
```


- Install Helm

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

helm version
```

# TASKS

### TASK 1

1. Creating a kubernetes cluster using kind

```bash
sudo su

kind create cluster # creates a single node cluster

kind get clusters # see all the created clusters

kubectl get nodes # to see all the nodes of clusters

# TO CREATE MULTI NODE CLUSTER
kind create cluster --name=multi-node-cluster --config=kind-config
```

2. Visualize using Promethius and Grafana

```bash
# adding prometheus repo
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update

# Create a Kubernetes Namespace
kubectl create namespace monitoring

# Echo username and password to a file
echo -n 'adminuser' > ./admin-user # change your username
echo -n 'p@ssword!' > ./admin-password # change your password

# Create a Kubernetes Secret
kubectl create secret generic grafana-admin-credentials --from-file=./admin-user --from-file=admin-password -n monitoring

rm admin-user && rm admin-password

# Create a values file to hold our helm values
touch values.yaml

# get the values from the below link
https://github.com/techno-tim/launchpad/blob/master/kubernetes/kube-prometheus-stack/values.yml

# Create our kube-prometheus-stack
helm install -n monitoring prometheus prometheus-community/kube-prometheus-stack -f values.yaml

# find your grafana pod name using
kubectl get pods --all-namespaces

# PORT forwarding from 3000:8080
kubectl port-forward -n monitoring your_grafana_pod_name 8080:3000

# Open and enter your secret that you created
http://localhost:8080
```

3. Increase traffic using Hey

```bash
#Install Hey
go install github.com/rakyll/hey@latest

hey http://10.96.0.1

# This opens 2 connections, and sends 10 requests. Each request is a form post with 2 parameters to your resource. Tweak the command to add/remove more flags as you need it.
```

# VIDEO TUTORIALS

### TASK 1

1. Kubernetes Cluster Setup with Kind
   https://youtu.be/DfmxNzbGPzY

2. Beautiful Dashboards with Grafana and Prometheus - Monitoring Kubernetes Tutorial
   https://youtu.be/fzny5uUaAeY

3. DOCS for creating dashboards with grafana and prometheus
   https://docs.technotim.live/posts/kube-grafana-prometheus/
