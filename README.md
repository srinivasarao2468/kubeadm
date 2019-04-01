# Kubeadm
### install  Vargrant and oracle Virtualbox on your laptop

### launch centos-7 VM's using Vagrantfile [Click Here](url of Vagrantfile).
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
### Comment swap UUID in /etc/fstab
```
$ vi /etc/fstab
```
### Enable br_netfilter Kernel Module
```
The br_netfilter module is required for kubernetes installation. Enable this kernel module so that the packets traversing the bridge are processed by iptables for filtering and for port forwarding, and the kubernetes pods across the cluster can communicate with each other.
```
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
