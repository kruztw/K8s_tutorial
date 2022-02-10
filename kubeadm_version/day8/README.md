# Install echo server with Helm


## What is Helm ?

[Document](https://helm.sh/)

## Install Helm

[Document](https://helm.sh/docs/intro/install/)

```shell=
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
``` 

## How to use ?

[Document](https://helm.sh/docs/intro/quickstart/)


## Create Chart for our echo server

```
helm create k8s_helm

# edit the values.yaml
# edit the templates/{deployment.yaml,service.yaml}

helm install echo-server ./k8s_helm/

# check
helm list
kubectl get all

# upgrade (if something changes)
helm upgrade echo-server ./k8s_helm/

# delete
helm uninstall echo-server
``` 


