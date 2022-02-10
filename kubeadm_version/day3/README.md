# Setup K8s nodes

My Environment:

* master node:
	* master: 192.168.11.100

* worker node:
	* node1: 192.168.11.101
	* node2: 192.168.11.102


## master node

```shell=
kubeadm init --pod-network-cidr=10.240.0.0/16 --apiserver-advertise-address 192.168.11.100
export KUBECONFIG=/etc/kubernetes/admin.conf

## remember kubeadm join command, it will be used in worker nodes.

## install CNI (Here, we use calico)
curl https://projectcalico.docs.tigera.io/manifests/calico.yaml -O

vim calico.yaml
# uncomment the following two line and change 192.168.0.0/16 to 10.240.0.0/16
- name: CALICO_IPV4POOL_CIDR
  value: "10.240.0.0/16"

kubectl create -f ./calico.yaml


## do worker node

## check nodes 
kubectl get nodes # make sure there are three nodes (master, node1, node2) and there statuses are all Ready
```

## worker node

```shell
kubeadm join 192.168.11.100:6443 --token <your_token> \
	--discovery-token-ca-cert-hash sha256:<your_sha256>
```

## note

1. How to recreate/rejoin K8s ?

kubeadm reset

2. What is CNI ?

[Document](https://github.com/containernetworking/cni)

3. What is calico ?

[Document](https://projectcalico.docs.tigera.io/getting-started/kubernetes/quickstart)

4. I forgot kubeadm join token

kubeadm token create --print-join-command   # or kubeadm token list

