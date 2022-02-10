# Create Service

## master node

```shell=
kubectl create -f ./deployment.yaml

# nodePort
kubectl create -f ./service.yaml

## check
kubectl get service # or kubectl get svc

### rsult
NAME           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
echo-service   NodePort    10.102.224.246   <none>        8888:30000/TCP   5s          <-- 8888: service port ; 30000: node port
kubernetes     ClusterIP   10.96.0.1        <none>        443/TCP          33m         <-- this is the default svc

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
IP:                       10.102.224.246
IPs:                      10.102.224.246
Port:                     <unset>  8888/TCP                                             <-- node1: <unset> ; node2: 8888 (run "nc 10.102.224.246 8888" in node2)
TargetPort:               1234/TCP
NodePort:                 <unset>  30000/TCP                                            <-- node1: <unset> ; node2: 30000 (run "nc 192.168.11.102 30000")
Endpoints:                10.240.104.2:1234,10.240.166.129:1234,10.240.166.130:1234     <-- three containers
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>

## delete
kubectl delete -f ./service.yaml
```

## TBD

* Load Balancer

* ClusterIP
