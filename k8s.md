---
title: kubernetes
description: 
published: true
date: 2019-07-11T12:34:41.641Z
tags: 
---

# Header

## minikube

### Start Minikube itself:

```
❯ minikube start
```

### status/check Minikube itself:

```
❯ minikube status

❯ kubectl cluster-info

❯ kubectl get nodes

❯ VBoxManage list runningvms
```


### Testing Deployments

```
❯ mkdir -p projects/nginx/cd projects/nginx/
```

#### Now create a new yaml file for our deployment configuration.

```
❯ vim nginx-deployment.yaml
```

#### And paste configurations below.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  labels:
    run: nginx-service
spec:
  type: NodePort
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: nginx
```

#### Now create the deployment by running the kubectl command below.

```
❯ kubectl create -f nginx-deployment.yaml
```




#### When it's complete, check the Kubernetes deployment.

```
❯ kubectl get deployments

❯ kubectl describe deployments nginx-deployment
```

#### Now check the Kubernetes service.

```
❯ kubectl get services

NAME             TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
hello-minikube   NodePort    10.103.17.160   <none>        8080:30798/TCP   5m
kubernetes       ClusterIP   10.96.0.1       <none>        443/TCP          23m
nginx-service    NodePort    10.98.112.245   <none>        80:31500/TCP     14m


❯ kubectl describe services nginx-service

Name:                     nginx-service
Namespace:                default
Labels:                   run=nginx-service
Annotations:              <none>
Selector:                 app=nginx
Type:                     NodePort
IP:                       10.98.112.245
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  31500/TCP
Endpoints:                172.17.0.4:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

```
❯ minikube service <name-of-service> --url
```

#### Check the Kubernetes cluster IP and access it using curl command.

```
❯ minikube ip
192.168.99.102

❯ curl -I http://192.168.99.102:31500/
```

#### Below is the result when we access from the web browser.

```
❯ http://<ip>:32274/
```

#### Access Kubernetes Dashboard

```
❯ minikube dashboard
```

---
---
---

#### Create a new pod:

```
❯ kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080
```

### Run the service:

```
❯ kubectl expose deployment hello-minikube --type=NodePort

# service "hello-minikube" exposed

```

### List pods:

```
❯ kubectl get pod
```

### And check the service itself:

```
❯ curl $(minikube service hello-minikube --url)
```
### SSH into the VM Minikube:

```
❯ minikube ssh
```

### Finally stop/delete Minikube:

```
❯ minikube stop && minikube delete
```

### Done.


---
