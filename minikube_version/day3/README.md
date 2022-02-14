# Create POD

## master node

```shell=
kubectl create -f ./POD.yaml

# check
kubectl get pods
kubectl describe pod <pod_name>

# another terminal
minikube ssh
docker ps -a | grep echo_server

# delete
kubectl delete -f ./POD.yaml
# or kubectl delete pod <pod_name> [--force]
```


## result

```
Name:         echoserver
Namespace:    default
Priority:     0
Node:         minikube/192.168.49.2                  # <--- this is minikube default node (this node works as master node and worker node)
Start Time:   Thu, 10 Feb 2022 21:03:09 +0800
Labels:       <none>
Annotations:  <none>
Status:       Running
IP:           172.17.0.2                         
IPs:
  IP:  172.17.0.2                                     # <--- container IP (run "nc 172.17.0.2 1234" in minikube node)
Containers:
  echo-server:
    Container ID:   docker://ce7cd08e21f6cc2b3c81da19a92ac376ad4b6b7665f566f65709c27800c72508
    Image:          kruztw/echo_server:latest
    Image ID:       docker-pullable://kruztw/echo_server@sha256:8627451e850f499adb38543061a6fe9853e9b78656a48741c2befa92e8f64c7d
    Port:           1234/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 10 Feb 2022 21:03:14 +0800
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-fcp76 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-fcp76:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  25s   default-scheduler  Successfully assigned default/echoserver to minikube
  Normal  Pulling    24s   kubelet            Pulling image "kruztw/echo_server:latest"
  Normal  Pulled     20s   kubelet            Successfully pulled image "kruztw/echo_server:latest" in 4.172375193s
  Normal  Created    20s   kubelet            Created container echo-server
  Normal  Started    20s   kubelet            Started container echo-server
  ```

