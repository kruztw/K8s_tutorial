# Create POD

## master node

```shell=
kubectl create -f ./POD.yaml

# check
kubectl get pods
kubectl describe pod <pod_name>


# delete
kubectl delete -f ./POD.yaml
# or kubectl delete pod <pod_name> [--force]
```


## result

```
Name:         echoserver
Namespace:    default
Priority:     0
Node:         node2/192.168.11.102              # <-----------   This pods is running on node2 (run "docker ps -a | grep echo_server" on node2)
Start Time:   Thu, 10 Feb 2022 01:22:19 +0800
Labels:       <none>
Annotations:  cni.projectcalico.org/containerID: 69441c921ad36818536133f5d9da4f2a164080e852960367472d995247896d74
              cni.projectcalico.org/podIP: 10.0.104.3/32
              cni.projectcalico.org/podIPs: 10.0.104.3/32
Status:       Running
IP:           10.0.104.3                        # <-----------   POD's IP (run "nc 10.0.104.3 1234" on node2)
IPs:
  IP:  10.0.104.3
Containers:
  echo-server:
    Container ID:   docker://218cd2f30051aff2e27f0411abd1d957639843da5c9025f0848219c31298bf25
    Image:          kruztw/echo_server:latest
    Image ID:       docker-pullable://kruztw/echo_server@sha256:8627451e850f499adb38543061a6fe9853e9b78656a48741c2befa92e8f64c7d
    Port:           1234/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 10 Feb 2022 01:22:24 +0800
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-s5qqm (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  kube-api-access-s5qqm:
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
  Normal  Scheduled  18s   default-scheduler  Successfully assigned default/echoserver to node2
  Normal  Pulling    16s   kubelet            Pulling image "kruztw/echo_server:latest"
  Normal  Pulled     13s   kubelet            Successfully pulled image "kruztw/echo_server:latest" in 2.647382729s
  Normal  Created    13s   kubelet            Created container echo-server
  Normal  Started    13s   kubelet            Started container echo-server
```
