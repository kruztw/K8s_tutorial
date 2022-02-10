# Setup Environment

## Install

[Document](https://minikube.sigs.k8s.io/docs/start/)


```
# run as root
apt install docker.io
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# vim ~/.bashrc (or download kubectl)
alias kubectl="minikube kubectl --"

source ~/.bashrc
```

## Test

```
minikube start
minikube ip
ping <minikube_ip>
minikube ssh
exit

# just make sure there are kubectl
kubectl get all
```
