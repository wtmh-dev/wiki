---
title: kubernetes
description: 
published: true
date: 2019-07-11T11:41:06.861Z
tags: 
---

# Header

## minikube

### Start Minikube itself:

```
$ minikube start
```

### Check Virtualbox VMs running:

```
$ VBoxManage list runningvms
"minikube" {37da5f2d-c486-4419-a152-eb5dcedde6f3}

```

### Create a new pod:

```
$ kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/hello-minikube created

```

### Run the service:

```
$ kubectl expose deployment hello-minikube --type=NodePort
service "hello-minikube" exposed

```

### List pods:

```
$ kubectl get pod
NAME                              READY   STATUS    RESTARTS   AGE
hello-minikube-5857d96c67-x46nk   1/1     Running   0          74s

```

### And check the service itself:

```
$ curl $(minikube service hello-minikube --url)
CLIENT VALUES:
client_address=172.17.0.1
command=GET
real path=/
query=nil
request_version=1.1
request_uri=http://192.168.99.100:8080/

SERVER VALUES:
server_version=nginx: 1.10.0 - lua: 10001

HEADERS RECEIVED:
accept=*/*
host=192.168.99.100:31345
user-agent=curl/7.64.0
BODY:
-no body in request-

```

### After you’ll finish – stop and delete VMs, pods, and services:

```
$ minikube stop
 [✋] Stopping "minikube" in virtualbox ...
 [🛑] "minikube" stopped.

```

### Done.


---
