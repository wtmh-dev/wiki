---
title: kubernetes
description: 
published: true
date: 2019-07-12T09:44:16.576Z
tags: 
---

## Definitions:

* **Node** : A compute instance which runs docker containers, managed by a cluster master.

.
* **Cluster** : One or more "worker nodes" which run containers. Very similar to a Docker Swarm node. In most cloud provider deployments, the [master node for your cluster is provided free of charge](https://www.sdxcentral.com/articles/news/google-eliminates-gke-management-fees-kubernetes-clusters/2017/11/), but you don't get to access it.

.
*   **Pod** : A collection of one or more the containers. If a pod runs multiple containers, these containers always run on the same node.

.
*   **Deployment** : A definition of a desired state. I.e., "I want a pod with containers A and B running". The Kubernetes master then ensures that any changes necessary to maintain the state are taken. (*I.e., if a pod crashes, but is supposed to be running, a new pod will be started*)

.
*   **Service** : Unlike Docker Swarm, service discovery is not *built in* to Kubernetes. For your pods to discover each other (say, to have "webserver" talk to "database"), you create a service for each pod, and refer to these services when you want your containers (*in pods*) to talk to each other. Complicated, yes, but the abstraction allows you to do powerful things, like auto\-scale\-up a bunch of database "pods" behind a service called "database", or perform a rolling container image upgrade with zero impact.

.
*   **External access** : Services not only allow pods to discover each other, but they're also the mechanism through which the outside world can talk to a container. At the simplest level, this is akin to exposing a container port on a docker host.

.
*   **Ingress** : When mapping ports to applications is inadequate (think virtual web hosts), an ingress is a sort of "inbound router" which can receive requests on one port (i.e., HTTPS), and forward them to a variety of internal pods, based on things like VHOST, etc. For us, this is the functional equivalent of what Traefik does in Docker Swarm. In fact, we use a Traefik Ingress in Kubernetes to accomplish the same.

.
*   **Persistent Volume** : A virtual disk which is attached to a pod, storing persistent data. Meets the requirement for shared storage from Docker Swarm. I.e., if a persistent volume (PV) is bound to a pod, and the pod dies and is recreated, or get upgraded to a new image, the PV the data is bound to the new container. PVs can be "claimed" in a YAML definition, so that your Kubernetes provider will auto\-create a PV when you launch your pod. PVs can be snapshotted.

.
*   **Namespace** : An abstraction to separate a collection of pods, services, ingresses, etc. A "virtual cluster within a cluster". Can be used for security, or simplicity. For example, since we don't have individual docker stacks anymore, if you commonly name your database container "db", and you want to deploy two applications which both use a database container, how will you name your services? Use namespaces to keep each application ("nextcloud" vs "kanboard") separate. Namespaces also allow you to allocate resources **limits** to the aggregate of containers in a namespace, so you could, for example, limit the "nextcloud" namespace to 2.3 CPUs and 1200MB RAM.

---
---
---

## minikube

### Start Minikube itself:

```
❯ minikube start
```

### change the nameSpace/context:

```
❯ kubens
# for change the nameSpace

❯ kubectx
# for change the context
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
#### delete the service/deployment

```
❯ kubectl delete services <name-of-service>

❯ kubectl delete deployment <name-of-service>
```

#### get the url

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
