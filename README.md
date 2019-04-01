# Kubeadm
### install  Vargrant and oracle Virtualbox on your laptop

### launch centos-7 VM's using [Vagrantfile](https://github.com/srinivasarao2468/kubeadm/blob/master/Vagrantfile).
## Root privileges
```
sudo su -
```
### Update your VM with below command
```
$ sudo yum update -y
```
### In case you don’t have your own dns server then update /etc/hosts file on master and worker nodes
```
$ vi /etc/hosts
```
```
192.168.1.10 master
192.168.1.20 worker1
192.168.1.30 worker2
```

### Run the command below to disable SELinux
```
$ sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux
```
### Run the command below to disable SELinux
```
$ cat /etc/sysconfig/selinux
```

###Identify configured swap devices and files with below command
```
$ cat /proc/swaps
```
### Disable the Swap memory
```
$ swapoff -a
```
### Comment swap UUID in /etc/fstab OR run the command to add # for swap in /etc/fstab
```
$ vi /etc/fstab
or
$ sudo sed -i.bak '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```
### Enable br_netfilter Kernel Module

The br_netfilter module is required for kubernetes installation. Enable this kernel module so that the packets traversing the bridge are processed by iptables for filtering and for port forwarding, and the kubernetes pods across the cluster can communicate with each other.

### Run the command below to enable the br_netfilter kernel module
```
$ modprobe br_netfilter
$ echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables
```
## Docker
### To uninstall old versions run the below command
```
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
## Install using the repository [Official Installation Guide](https://docs.docker.com/install/linux/docker-ce/centos/).
### SET UP THE REPOSITORY

```
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```
### Use the following command to set up the stable repository.
```
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```
### TO install docker specfic version
```
Syntax:- $ yum install -y docker-ce-<Version> containerd.io

$ yum install -y docker-ce-18.06.0.ce-3.el7 containerd.io
```
### Add kubernetes to yum repository so that we can use our package manager to install latest version of kubernetes [Official Installation Guide](https://kubernetes.io/docs/setup/independent/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl).
```
$ cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF
```

### Now install the kubernetes packages kubeadm, kubelet, and kubectl using the yum command below
```
$ yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
```
### After the installation is complete, restart all those servers
```
sudo reboot
```
### Again login to servers and start the services of docker and kubelet
```
$ systemctl start docker && systemctl enable docker
$ systemctl start kubelet && systemctl enable kubelet
```
## Change the cgroup-driver
#### We need to make sure the docker-ce and kubernetes are using same 'cgroup'.
### Check docker cgroup using the docker info command.
```
$ docker info | grep -i cgroup
```
And you see the docker is using 'cgroupfs' as a cgroup-driver.

### Now run the command below to change the kuberetes cgroup-driver to 'cgroupfs'.
```
$ sed -i 's/cgroup-driver=systemd/cgroup-driver=cgroupfs/g' /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```

### To intialize the kubeadm cluster
```
kubeadm init --apiserver-advertise-address=192.168.1.10 --pod-network-cidr=192.168.0.0/16
```
### If you found any error related to cpu run below command because it needs 2 cpus
```
 kubeadm init --apiserver-advertise-address=192.168.1.10 --pod-network-cidr=192.168.0.0/16 --ignore-preflight-errors=NumCPU
 ```
