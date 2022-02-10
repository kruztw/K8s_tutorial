# Create Ingress & NGINX Ingress Controller

## What is Ingress & Ingress Controller (IC) ?

[Tutorial](https://www.youtube.com/watch?v=80Ew_fsV4rM&t=1102s&ab_channel=TechWorldwithNana)

## Install  

[Document](https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/)

`Add NGINX Ingress repo`

```shell=
minikube addons enable ingress

# check
kubectl get pods -n ingress-nginx
```

### Edit NGINX Ingress Controller Deployment

```shell=
kubectl edit deployments -n ingress-nginx ingress-nginx-controller

    spec:
      containers:
      - args:
        - /nginx-ingress-controller
        - --publish-service=$(POD_NAMESPACE)/ingress-nginx-controller     # <--- add this line
        - --election-id=ingress-controller-leader
        - --ingress-class=nginx                                           # <--- add this line
        - --configmap=$(POD_NAMESPACE)/ingress-nginx-controller
        - --tcp-services-configmap=$(POD_NAMESPACE)/tcp-services          # <--- add this line
        - --validating-webhook=:8443
        - --validating-webhook-certificate=/usr/local/certificates/cert
        - --validating-webhook-key=/usr/local/certificates/key
```

### Install Echo Server

```shell=
helm install echo-server ./k8s_helm
``` 

```shell=
kubectl patch configmap tcp-services -n ingress-nginx --patch '{"data":{"7777":"default/echo-server:8888"}}'
```

## Add ports to NGINX Ingress Controller Deployment

```shell=
kubectl patch deployment ingress-nginx-controller -n ingress-nginx --patch "$(cat k8s_helm/patch/nginx-ingress-controller-patch.yaml)"
```

### Add ports to NGINX Ingress Controller Service

```shell=
kubectl patch service ingress-nginx-controller -n ingress-nginx --patch "$(cat k8s_helm/patch/nginx-ingress-svc-controller-patch.yaml)"
```

### TEST

```
kubectl get service -n ingress-nginx ingress-nginx-controller

# result:
NAME                       TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)                                     AGE
ingress-nginx-controller   NodePort   10.103.52.255   <none>        8888:31100/TCP,80:32175/TCP,443:31630/TCP   6m18s

kubectl port-forward svc/ingress-nginx-controller -n ingress-nginx 6666:8888
nc localhost 6666
```

## Clean up

```shell=
helm uninstall echo-server
```
