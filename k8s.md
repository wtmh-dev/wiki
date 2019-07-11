---
title: kubernetes
description: 
published: true
date: 2019-07-11T11:54:43.370Z
tags: 
---

# Header

## minikube

### Start Minikube itself:

```
minikube start
```

### status/check Minikube itself:

```
minikube status
kubectl cluster-info
kubectl get nodes
VBoxManage list runningvms
```

### Create a new pod:

```
$ kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080
```

### Run the service:

```
kubectl expose deployment hello-minikube --type=NodePort
# service "hello-minikube" exposed

```

### List pods:

```
kubectl get pod
```

### And check the service itself:

```
curl $(minikube service hello-minikube --url)
```
### SSH into the VM Minikube:

```
minikube ssh
```

### Finally stop/delete Minikube:

```
minikube stop
minikube delete
```

### Done.


---
