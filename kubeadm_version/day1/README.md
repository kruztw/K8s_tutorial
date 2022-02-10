# Setup Environment

## master node (hostname: master)

bare metal:

* ubuntu: 20.04 

## 2 worker nodes (hostname: node1, node2)

virtual box:

spec: (Optional)

* ubuntu: 20.04 [iso download url](https://ubuntu.com/download/desktop) (required)

* CPU: 4

* Memory: 8 GB 

* disk: 50GB

* Network: 
	* Bridged Adapter (required: make sure each nodes can ping others)
	* NAT (Optional: maybe your bridged network can't access IPV6 like mine, and don't forget to reconfigure routing table)	
		* if both nodes use the same NAT ip, you have to change one IP address
		* e.g.
		* # In my case, enp0s8 is the NAT interface and enp0s3 is the bridged interface
		* ifconfig enp0s3 down
                * ifconfig enp0s3 192.168.11.101/24           # change node1 ip address
                * ifconfig enp0s3 up
		* ifconfig enp0s8 10.0.11.16/24
		* route add -net 10.0.3.0 netmask 255.255.255.0 dev enp0s8
		* route add default gw 10.0.3.2 dev enp0s8


## Install (each nodes)

[document](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

```shell=
# run as root

## make virtual box full screen
sudo apt update
sudo apt install linux-headers-$(uname -r) build-essential dkms
click Devices in nav bar > Insert Guest Additions CD Image...
Devices > Shared Clipboard > Bidirectional
reboot

## install tools
sudo apt install docker.io vim net-tools
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl


# disable swap space
vim /etc/fstab
## comment /swapfile likes below
# /swapfile ...


# sometimes kubelet may cause error
# note: This is not the best solution but works
cat > /etc/docker/daemon.json <<EOF
{"exec-opts": ["native.cgroupdriver=systemd"]}
EOF

systemctl restart docker
```







