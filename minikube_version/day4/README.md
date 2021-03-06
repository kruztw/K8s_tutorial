# Create Deployment

## master node

```shell=
kubectl create -f ./deployment.yaml

# check
kubectl get deployment
kubectl describe deployment echoserver   

# test
kubectl get pods
kubectl delete pod <running_pod>
kubectl get pods                    # still has three pods (3 running) but the pod name is different

# delete
kubectl delete -f ./deployment.yaml
# or kubectl delete deployment <deployment_name>

``` 

## result

```
Name:                   echoserver
Namespace:              default
CreationTimestamp:      Thu, 10 Feb 2022 21:10:50 +0800
Labels:                 app=server
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=server
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=server
  Containers:
   echo-server:
    Image:        kruztw/echo_server:latest
    Port:         1234/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   echoserver-74ddd95b85 (3/3 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  21s   deployment-controller  Scaled up replica set echoserver-74ddd95b85 to 3
```
