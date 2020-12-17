## Namespaces[Official guide](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/#creating-a-new-namespace):
it's an abstraction used by Kubernetes to support multiple virtual clusters on same physical cluster.

Kubernetes starts with 3 initial namespaces:
* **default** The default namespace for objects with no other namespace.
* **kube-system** The namespace for objects created by the Kubernetes system.
* **kube-public** his namespace is created automatically and is readable by all users (including those not  authenticated). This namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable publicly throughout the whole cluster. The public aspect of this namespace is only a convention, not a requirement.

**Note:** Avoid creating namespace with prefix kube-, since it is reserved for Kubernetes system namespaces

#### A namespace can be in one of two phases:
* Active the namespace is in use
* Terminating the namespace is being deleted, and can not be used for new objects

### List the current namespaces in a cluster using.
```
$ kubectl get namespaces name
```
### Create namespace in cluster using [namespace.yaml](https://raw.githubusercontent.com/srinivasarao2468/kubeadm/master/Namespace/namespace.yaml)
```
$ kubectl create namespaces name
or
$ kubectl create -f namespace.yaml
```
### To get detailed information of namespace
```
$ kubectl describe namespaces name
```
### delete namespace in cluster using [namespace.yaml](https://raw.githubusercontent.com/srinivasarao2468/kubeadm/master/Namespace/namespace.yaml)
```
$ kubectl delete namespaces name
or
$ kubectl create -f [namespace.yaml](https://raw.githubusercontent.com/srinivasarao2468/kubeadm/master/Namespace/namespace.yaml)
```
