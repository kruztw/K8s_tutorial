# Create Ingress & NGINX Ingress Controller

## What is Ingress & Ingress Controller (IC) ?

[Tutorial](https://www.youtube.com/watch?v=80Ew_fsV4rM&t=1102s&ab_channel=TechWorldwithNana)

## Before starting

[Ingress Document](https://kubernetes.io/docs/concepts/services-networking/ingress/)

[nginx ingress controller Document](https://kubernetes.github.io/ingress-nginx/deploy/#bare-metal-clusters)

Publishing echo service with IC is more complicated than publishing HTTP services with IC, since there are additional configuraions in IC.

I strongly recommend the readers to study the above documents, and publish an HTTP service with IC first.


## Install  

`Add NGINX Ingress repo`

```shell=
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

`Install NGINX Ingress on ingress-nginx namespace`

```shell=
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.1/deploy/static/provider/baremetal/deploy.yaml
kubectl delete -A ValidatingWebhookConfiguration ingress-nginx-admission
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

# Don't forget change IP in k8s_helm/patch/nginx-ingress-svc-controller-patch.yaml
kubectl patch service ingress-nginx-controller -n ingress-nginx --patch "$(cat k8s_helm/patch/nginx-ingress-svc-controller-patch.yaml)"
```

### TEST

```
kubectl get service -n ingress-nginx ingress-nginx-controller

# result:
NAME                       TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)                                     AGE
ingress-nginx-controller   NodePort   10.97.211.84   <none>        8888:31100/TCP,80:31474/TCP,443:30361/TCP   107m

# 

kubectl get ing --watch
NAME          CLASS   HOSTS              ADDRESS        PORTS   AGE
echo-server   nginx   echo-server.kube   10.97.211.84   80      107m 

vim /etc/hosts/  # add
echo-server.kube <node1_ip> # or <node2_ip>, use "kubectl describe svc ingress-nginx-controller -n ingress-nginx" to check 
nc echo-server.kube 31100
```

## Clean up

```shell=
# remove echo server
helm uninstall echo-server
helm uninstall ingress-nginx -n ingress-nginx

# remove nginx repo
helm repo remove ingress-nginx
```

## Issue

1. Why not using externalIPs to expose service to specific node ?

I tried, but failed.

As you can see, I add the externalIPs in patch file, however I can't access echo server with command "nc 192.168.11.101 8888" runs in worker node.


