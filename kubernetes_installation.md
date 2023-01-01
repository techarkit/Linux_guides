## How to install Kubernetes Cluster in Centos 7 Step by Step Guide

I am using this Centos ISO image for the Kube Node and worker Nodes

[Download Centos 7 ISO](http://repo.extreme-ix.org/centos/7.9.2009/isos/x86_64/CentOS-7-x86_64-Minimal-2009.iso)

## Hardware Requirement
### Kubernet Master required ###
- Atleast 2 CPU Cores
- 4GB RAM 
- 20GB HDD (Minimum)
### Kube Nodes Required ###
- Atleast 2 CPU Cores
- 4GB RAM
- 40GB HDD (Minimum)

### Update OS packges and Security patches run this steps in all the nodes

```
yum update -y

sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
setenforce 0
reboot
```

### Add Firewall rules if your firewalld is running (You can skip if not running)
```
firewall-cmd --permanent --add-port=6443/tcp
firewall-cmd --permanent --add-port=2379-2380/tcp
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=10251/tcp
firewall-cmd --permanent --add-port=10252/tcp
firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd –reload
modprobe br_netfilter
echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
```

### Update iptable Settings
```
cat <<EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl --system
```

### Remove Older Docker if already installed
```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine 
```

### Install yum utilities
```	
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin git wget -y
sudo systemctl enable docker && sudo systemctl start docker
sudo systemctl enable containerd && sudo systemctl start containerd

```	

### Create YUM repository to install required packages
```	
vi /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
```	

### Install Kubeadm and Docker packages

```	
yum install kubeadm docker -y 

systemctl enable kubelet && systemctl start kubelet

kubeadm init
```	

### If you get below error delete the config.toml file and restart containerd service
```
[preflight] Running pre-flight checks
        [WARNING Hostname]: hostname "kubemaster.techarkit.local" could not be reached
        [WARNING Hostname]: hostname "kubemaster.techarkit.local": lookup kubemaster.techarkit.local on 192.168.175.2:53: no such host
error execution phase preflight: [preflight] Some fatal errors occurred:
        [ERROR CRI]: container runtime is not running: output: E1211 20:58:19.685923  104550 remote_runtime.go:948] "Status from runtime service failed" err="rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
        
[root@kubemaster ~]#  rm /etc/containerd/config.toml
rm: cannot remove ‘/etc/containerd/config.toml’: No such file or directory
[root@kubemaster ~]# systemctl restart containerd
```	

```
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config
export KUBECONFIG=/etc/kubernetes/admin.conf

kubectl get nodes --all-namespaces
```

### Example Command 
```	
kubeadm join 192.168.117.200:6443 --token 869h67.4x0irxa14419ep1s \
        --discovery-token-ca-cert-hash sha256:da5ed4a9ec779f8b1eca198e8d6fe7b5cf8c29fa8f1f512b0099a808b17759bc
```	

### Add Kube Flannel to create Networking between all the Nodes
```	
git clone https://github.com/techarkit/Linux_guides.git
cd Linux_guides/
sudo kubectl apply -f /root/Linux_guides/kube-flannel.yml

OR

sudo kubectl apply -f https://raw.githubusercontent.com/techarkit/Linux_guides/master/kube-flannel.yml
```	

```
# kubectl get nodes
NAME                         STATUS   ROLES           AGE    VERSION
kubemaster.techarkit.local   Ready    control-plane   153m   v1.26.0
kubenode1.techarkit.local    Ready    <none>          25m    v1.26.0
```

# Adding KubeNode (Worker Node to Cluster)

### Disable SELinux and Update packages
```
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
setenforce 0
yum update -y
reboot
```
### Add Host Entries for local resolution
```
vi /etc/hosts
192.168.175.200 kubemaster.techarkit.local kubemaster
192.168.175.201 kubenode1.techarkit.local kubenode1
192.168.175.202 kubenode2.techarkit.local kubenode2
:wq
```

### After Reboot check SELinux Status
```
sestatus
SELinux status:                 disabled
```

### Add Firewall Rules 
```
firewall-cmd --permanent --add-port=6783/tcp
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd --permanent --add-port=30000-32767/tcp
firewall-cmd --reload
echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
```

## Add Kubernetes Repository to Install Kubeadm packages
```
vi /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg

:wq
```
### Add Docker Repository to Install container run-time
```
yum install yum-utils -y
yum-config-manager     --add-repo     https://download.docker.com/linux/centos/docker-ce.repo

yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin git wget -y
yum install kubeadm -y

systemctl enable docker && systemctl start docker
systemctl enable kubelet && systemctl start kubelet
```

### Join Node to Kubernetes Cluster 
```
kubeadm join 192.168.175.200:6443 --token vaki2g.wevj9qs1d6unyj0k         --discovery-token-ca-cert-hash sha256:68c74acbbcea624e2cdbeee4acc1a3feb96f58a53b643476b9f77769e340b98b
```

### If you get below error like Kubelet is not running
```
systemctl status kubelet -l

---------------------------- Error ------------------------------------
		 Loaded: loaded (/usr/lib/systemd/system/kubelet.service; enabled; vendor preset: disabled)
  Drop-In: /usr/lib/systemd/system/kubelet.service.d
           └─10-kubeadm.conf
   Active: activating (auto-restart) (Result: exit-code) since Sun 2023-01-01 21:23:01 IST; 3s ago
     Docs: https://kubernetes.io/docs/
  Process: 9815 ExecStart=/usr/bin/kubelet $KUBELET_KUBECONFIG_ARGS $KUBELET_CONFIG_ARGS $KUBELET_KUBEADM_ARGS $KUBELET_EXTRA_ARGS (code=exited, status=1/FAILURE)
 Main PID: 9815 (code=exited, status=1/FAILURE)

Jan 01 21:23:01 kubenode1.techarkit.local systemd[1]: Unit kubelet.service entered failed state.
---------------------------- Error ------------------------------------

mv /etc/kubernetes/kubelet.conf /etc/kubernetes/admin.conf
```

### Set Variables and Re-run the Node Join command
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

systemctl restart kubelet

kubeadm join 192.168.175.200:6443 --token vaki2g.wevj9qs1d6unyj0k         --discovery-token-ca-cert-hash sha256:68c74acbbcea624e2cdbeee4acc1a3feb96f58a53b643476b9f77769e340b98b
```

### If you get ca.crt alreay exists error then delete the file
```
mv /etc/kubernetes/pki/ca.crt /tmp/
```

Re-run the Node Join Command
