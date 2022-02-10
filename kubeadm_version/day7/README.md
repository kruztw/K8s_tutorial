# Create Namespace

## What is Namespace ?

[Document](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

## master node

```shell=
kubectl create ns my-ns

# check
kubectl get ns
kubectl create -f ./deployment.yaml
kubectl create -f ./service.yaml
kubectl get all -n my-ns

# delete
kubectl delete ns my-ns
```

## note

kubectl get all -A  # -A <==> --all-namespace
