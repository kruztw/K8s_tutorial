# Create Service

## master node

```shell=
kubectl create -f ./deployment.yaml

# nodePort
kubectl create -f ./service.yaml

## check
kubectl get service # or kubectl get svc

### rsult
NAME           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
echo-service   NodePort    10.105.55.151   <none>        8888:30000/TCP   5s          <-- 8888: service port ; 30000: node port
kubernetes     ClusterIP   10.96.0.1       <none>        443/TCP          18m         <-- this is the default svc

kubectl describe svc echo-service

### result
Name:                     echo-service
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=server
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.105.55.151                                         <--- run "nc 10.105.55.151 8888" in minikube node
IPs:                      10.105.55.151
Port:                     <unset>  8888/TCP                                     <--- service port
TargetPort:               1234/TCP                                              <--- POD port
NodePort:                 <unset>  30000/TCP                                    <--- run "nc `minikube ip` 30000"
Endpoints:                172.17.0.12:1234,172.17.0.14:1234,172.17.0.15:1234    <--- three containers
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>

## delete
kubectl delete -f ./service.yaml
```

## TBD

* Load Balancer

* ClusterIP
