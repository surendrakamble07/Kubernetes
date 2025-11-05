# Kubernetes

# KIND Cluster Setup Guide

## 1. Installing KIND and kubectl
Install KIND and kubectl using the provided [script](https://github.com/LondheShubham153/kubestarter/blob/main/kind-cluster/install.sh):

## 2. Setting Up the KIND Cluster
Create a kind-config.yaml file:

```yaml

kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    image: kindest/node:v1.33.1
  - role: worker
    image: kindest/node:v1.33.1
  - role: worker
    image: kindest/node:v1.33.1
    extraPortMappings:
    - containerPort: 80
      hostPort: 80
      protocol: TCP
    - containerPort: 443
      hostPort: 443
      protocol: TCP
```
Create the cluster using the configuration file:

```bash

kind create cluster --config kind-config.yaml --name tws-kind-cluster
```
Verify the cluster:

```bash

kubectl get nodes
kubectl cluster-info
```
## 3. Accessing the Cluster
Use kubectl to interact with the cluster:
```bash

kubectl cluster-info
```


## 4. Setting Up the Kubernetes Dashboard
Deploy the Dashboard
Apply the Kubernetes Dashboard manifest:
```bash

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```
Create an Admin User
Create a dashboard-admin-user.yml file with the following content:

```yaml

apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```
Apply the configuration:

```bash

kubectl apply -f dashboard-admin-user.yml
```
Get the Access Token
Retrieve the token for the admin-user:

```bash

kubectl -n kubernetes-dashboard create token admin-user
```
Copy the token for use in the Dashboard login.

Access the Dashboard
Start the Dashboard using kubectl proxy:

```bash

kubectl proxy
```
Open the Dashboard in your browser:

```bash

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```
Use the token from the previous step to log in.

## 5. Deleting the Cluster
Delete the KIND cluster:
```bash

kind delete cluster --name my-kind-cluster
```

## 6. Notes

Multiple Clusters: KIND supports multiple clusters. Use unique --name for each cluster.
Custom Node Images: Specify Kubernetes versions by updating the image in the configuration file.
Ephemeral Clusters: KIND clusters are temporary and will be lost if Docker is restarted.
















Microsoft Windows [Version 10.0.26200.7019]
(c) Microsoft Corporation. All rights reserved.

C:\Users\suren>cd Downloads

C:\Users\suren\Downloads>ssh -i "k8s.pem" ubuntu@ec2-98-93-24-171.compute-1.amazonaws.com
Welcome to Ubuntu 24.04.3 LTS (GNU/Linux 6.14.0-1011-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Wed Nov  5 17:35:56 UTC 2025

  System load:  0.51               Processes:             170
  Usage of /:   22.3% of 28.02GB   Users logged in:       0
  Memory usage: 23%                IPv4 address for enX0: 172.31.22.146
  Swap usage:   0%


Expanded Security Maintenance for Applications is not enabled.

62 updates can be applied immediately.
33 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

1 additional security update can be applied with ESM Apps.
Learn more about enabling ESM Apps service at https://ubuntu.com/esm


Last login: Fri Oct 31 07:09:26 2025 from 106.221.223.48
ubuntu@ip-172-31-22-146:~$ sudo su
root@ip-172-31-22-146:/home/ubuntu# kubectl
kubectl controls the Kubernetes cluster manager.

 Find more information at: https://kubernetes.io/docs/reference/kubectl/

Basic Commands (Beginner):
  create          Create a resource from a file or from stdin
  expose          Take a replication controller, service, deployment or pod and expose it as a new Kubernetes service
  run             Run a particular image on the cluster
  set             Set specific features on objects

Basic Commands (Intermediate):
  explain         Get documentation for a resource
  get             Display one or many resources
  edit            Edit a resource on the server
  delete          Delete resources by file names, stdin, resources and names, or by resources and label selector

Deploy Commands:
  rollout         Manage the rollout of a resource
  scale           Set a new size for a deployment, replica set, or replication controller
  autoscale       Auto-scale a deployment, replica set, stateful set, or replication controller

Cluster Management Commands:
  certificate     Modify certificate resources
  cluster-info    Display cluster information
  top             Display resource (CPU/memory) usage
  cordon          Mark node as unschedulable
  uncordon        Mark node as schedulable
  drain           Drain node in preparation for maintenance
  taint           Update the taints on one or more nodes

Troubleshooting and Debugging Commands:
  describe        Show details of a specific resource or group of resources
  logs            Print the logs for a container in a pod
  attach          Attach to a running container
  exec            Execute a command in a container
  port-forward    Forward one or more local ports to a pod
  proxy           Run a proxy to the Kubernetes API server
  cp              Copy files and directories to and from containers
  auth            Inspect authorization
  debug           Create debugging sessions for troubleshooting workloads and nodes
  events          List events

Advanced Commands:
  diff            Diff the live version against a would-be applied version
  apply           Apply a configuration to a resource by file name or stdin
  patch           Update fields of a resource
  replace         Replace a resource by file name or stdin
  wait            Experimental: Wait for a specific condition on one or many resources
  kustomize       Build a kustomization target from a directory or URL

Settings Commands:
  label           Update the labels on a resource
  annotate        Update the annotations on a resource
  completion      Output shell completion code for the specified shell (bash, zsh, fish, or powershell)

Subcommands provided by plugins:

Other Commands:
  api-resources   Print the supported API resources on the server
  api-versions    Print the supported API versions on the server, in the form of "group/version"
  config          Modify kubeconfig files
  plugin          Provides utilities for interacting with plugins
  version         Print the client and server version information

Usage:
  kubectl [flags] [options]

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
root@ip-172-31-22-146:/home/ubuntu# kubectl get pods
No resources found in default namespace.
root@ip-172-31-22-146:/home/ubuntu# ls
install_kind.sh  kind_cluster
root@ip-172-31-22-146:/home/ubuntu# cd kind_cluster
root@ip-172-31-22-146:/home/ubuntu/kind_cluster# ls
config.yml
root@ip-172-31-22-146:/home/ubuntu/kind_cluster# cd . .
bash: cd: too many arguments
root@ip-172-31-22-146:/home/ubuntu/kind_cluster# cd ..
root@ip-172-31-22-146:/home/ubuntu# kubectl get ns
NAME                 STATUS   AGE
default              Active   5d10h
kube-node-lease      Active   5d10h
kube-public          Active   5d10h
kube-system          Active   5d10h
local-path-storage   Active   5d10h
root@ip-172-31-22-146:/home/ubuntu# kubectl get -n kube-system
You must specify the type of resource to get. Use "kubectl api-resources" for a complete list of supported resources.

error: Required resource not specified.
Use "kubectl explain <resource>" for a detailed description of that resource (e.g. kubectl explain pods).
See 'kubectl get -h' for help and examples
root@ip-172-31-22-146:/home/ubuntu# kubectl get pod -n kube-system
NAME                                                READY   STATUS        RESTARTS      AGE
coredns-674b8bbfcf-88jwn                            1/1     Running       0             8m43s
coredns-674b8bbfcf-8x55l                            1/1     Terminating   0             5d10h
coredns-674b8bbfcf-kc6hr                            1/1     Terminating   0             5d10h
coredns-674b8bbfcf-m4gpg                            1/1     Running       0             8m43s
etcd-tws-cluster-control-plane                      1/1     Running       0             5d10h
kindnet-88vkn                                       1/1     Running       0             5d10h
kindnet-8r6lk                                       1/1     Running       1 (15m ago)   5d10h
kindnet-xp22j                                       1/1     Running       1 (15m ago)   5d10h
kube-apiserver-tws-cluster-control-plane            1/1     Running       0             5d10h
kube-controller-manager-tws-cluster-control-plane   1/1     Running       0             5d10h
kube-proxy-95gdd                                    1/1     Running       1 (15m ago)   5d10h
kube-proxy-gljtm                                    1/1     Running       0             5d10h
kube-proxy-m2kj7                                    1/1     Running       1 (15m ago)   5d10h
kube-scheduler-tws-cluster-control-plane            1/1     Running       0             5d10h
root@ip-172-31-22-146:/home/ubuntu# kubectl create ns nginx
namespace/nginx created
root@ip-172-31-22-146:/home/ubuntu# ls
install_kind.sh  kind_cluster
root@ip-172-31-22-146:/home/ubuntu# kubectl get pod
No resources found in default namespace.
root@ip-172-31-22-146:/home/ubuntu# kubectl run nginx:nginx -n nginx
error: required flag(s) "image" not set
root@ip-172-31-22-146:/home/ubuntu# kubectl run --image=nginx -n nginx
error: NAME is required for run
See 'kubectl run -h' for help and examples
root@ip-172-31-22-146:/home/ubuntu# kubectl run nginx --image=nginx -n nginx
pod/nginx created
root@ip-172-31-22-146:/home/ubuntu# kubectl get pods
No resources found in default namespace.
root@ip-172-31-22-146:/home/ubuntu# kubectl get pods -n nginx
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          18s
root@ip-172-31-22-146:/home/ubuntu# kubectl delete pod nginx
Error from server (NotFound): pods "nginx" not found
root@ip-172-31-22-146:/home/ubuntu# kubectl delete pod nginx -n nginx
pod "nginx" deleted from nginx namespace
root@ip-172-31-22-146:/home/ubuntu# ns
Command 'ns' not found, but can be installed with:
apt install ns2
root@ip-172-31-22-146:/home/ubuntu# ls
install_kind.sh  kind_cluster
root@ip-172-31-22-146:/home/ubuntu# kubectl delete ns nginx
namespace "nginx" deleted
root@ip-172-31-22-146:/home/ubuntu# mkdir nginx
root@ip-172-31-22-146:/home/ubuntu# cd nginx
root@ip-172-31-22-146:/home/ubuntu/nginx#
root@ip-172-31-22-146:/home/ubuntu/nginx# vim namespace.yml
root@ip-172-31-22-146:/home/ubuntu/nginx# kubectl apply -f namespace.yml
namespace/nginx created
root@ip-172-31-22-146:/home/ubuntu/nginx# vim namespace.yml
root@ip-172-31-22-146:/home/ubuntu/nginx# vim pod.yml
root@ip-172-31-22-146:/home/ubuntu/nginx# kubectl apply -f pod.yml
pod/nginx created
root@ip-172-31-22-146:/home/ubuntu/nginx# kubectl get pods -n nginx
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          14s
root@ip-172-31-22-146:/home/ubuntu/nginx# kubect -it exec pod/pod -n nginx -- bash
Command 'kubect' not found, did you mean:
  command 'kubectx' from deb kubectx (0.9.5-1ubuntu0.4)
Try: apt install <deb name>
root@ip-172-31-22-146:/home/ubuntu/nginx# kubectl -it exec pod/pod -n nginx -- bash
Error from server (NotFound): pods "pod" not found
root@ip-172-31-22-146:/home/ubuntu/nginx# kubectl -it exec pod/Pod -n nginx -- bash
Error from server (NotFound): pods "Pod" not found
root@ip-172-31-22-146:/home/ubuntu/nginx# vim pod.yml
root@ip-172-31-22-146:/home/ubuntu/nginx# kubectl -it exec pod/nginx-pod -n nginx -- bash
Error from server (NotFound): pods "nginx-pod" not found
root@ip-172-31-22-146:/home/ubuntu/nginx# vim pod.yml
root@ip-172-31-22-146:/home/ubuntu/nginx# kubectl exec -it pod/nginx-pod -n nginx -- bash
Error from server (NotFound): pods "nginx-pod" not found
root@ip-172-31-22-146:/home/ubuntu/nginx# vim pod.yml
root@ip-172-31-22-146:/home/ubuntu/nginx# kubectl exec -it nginx-pod -n nginx -- bash
Error from server (NotFound): pods "nginx-pod" not found
root@ip-172-31-22-146:/home/ubuntu/nginx# vim pod.yml
root@ip-172-31-22-146:/home/ubuntu/nginx# kubectl apply -f pod.yml
pod/nginx-pod created
root@ip-172-31-22-146:/home/ubuntu/nginx# kubectl exec -it nginx-pod -n nginx -- bash
root@nginx-pod:/# ls
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc          product_uuid  run   srv  tmp  var
boot  docker-entrypoint.d  etc                   lib   media  opt  product_name  root          sbin  sys  usr
root@nginx-pod:/# curl 127.0.0.1
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@nginx-pod:/# exit
exit
root@ip-172-31-22-146:/home/ubuntu/nginx#
