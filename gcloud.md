---
title: Google-Cloud
description: gcloud commands
published: true
date: 2019-07-10T20:55:17.245Z
tags: 
---

# [google cloud architect certification](https://cloud.google.com/certification/cloud-architect)

## [Cloud Architecture](https://www.qwiklabs.com/quests/24)

### Orchestrating the Cloud with Kubernetes

### Deployment Manager - Full Production

### Continuous Delivery with Jenkins in Kubernetes Engine

### Networking 102

### Site Reliability Troubleshooting with Stackdriver APM

[19 state of salaries](https://hired.com/page/state-of-salaries)

### 	Set Up Network and HTTP Load Balancers

    https://cloud.google.com/compute/docs/zones
    gcloud config set compute/zone us-central1-a
    gcloud config set compute/region us-central1

    cat << EOF > startup.sh
    #! /bin/bash
    apt-get update
    apt-get install -y nginx
    service nginx start
    sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
    EOF

    gcloud compute instance-templates create nginx-template \
             --metadata-from-file startup-script=startup.sh

    gcloud compute target-pools create nginx-pool

    gcloud compute instance-groups managed create nginx-group \
             --base-instance-name nginx \
             --size 2 \
             --template nginx-template \
             --target-pool nginx-pool

    gcloud compute instances list
    gcloud compute firewall-rules create www-firewall --allow tcp:80


    gcloud compute forwarding-rules create nginx-lb \
             --region us-central1 \
             --ports=80 \
             --target-pool nginx-pool

    gcloud compute forwarding-rules list


    gcloud compute http-health-checks create http-basic-check


    gcloud compute instance-groups managed \
           set-named-ports nginx-group \
           --named-ports http:80

    gcloud compute backend-services create nginx-backend \
          --protocol HTTP --http-health-checks http-basic-check --global

    gcloud compute backend-services add-backend nginx-backend \
        --instance-group nginx-group \
        --instance-group-zone us-central1-a \
        --global

    gcloud compute url-maps create web-map \

        --default-service nginx-backend
    gcloud compute target-http-proxies create http-lb-proxy \
        --url-map web-map

    gcloud compute forwarding-rules create http-content-rule \
            --global \
            --target-http-proxy http-lb-proxy \
            --ports 80

    gcloud compute forwarding-rules list

+ True ← Network Load Balancing is a regional, non-proxied load balancer

---

### Orchestrating the Cloud with Kubernetes

	gcloud auth list
	gcloud config list project
 	gcloud config set compute/zone us-central1-b
	gcloud container clusters create io	
	git clone https://github.com/googlecodelabs/orchestrate-with-kubernetes.git
	cd orchestrate-with-kubernetes/kubernetes
	ls
	# Quick Kubernetes Demo
	kubectl create deployment nginx --image=nginx:1.10.0
	kubectl get pods
	kubectl expose deployment nginx --port 80 --type LoadBalancer
	kubectl get services
	curl http://<External IP>:80
	cat pods/monolith.yaml
	kubectl create -f pods/monolith.yaml
	kubectl describe pods monolith
	kubectl port-forward monolith 10080:80
	curl http://127.0.0.1:10080
	TOKEN=$(curl http://127.0.0.1:10080/login -u user|jq -r '.token')
	curl -H "Authorization: Bearer $TOKEN" http://127.0.0.1:10080/secure
	kubectl logs monolith
	kubectl exec monolith --stdin --tty -c monolith /bin/sh
	ping -c 3 google.com
	exit
	# Creating a Service
	cd ~/orchestrate-with-kubernetes/kubernetes
	cat pods/secure-monolith.yaml
	kubectl create secret generic tls-certs --from-file tls/
	kubectl create configmap nginx-proxy-conf --from-file nginx/proxy.conf
	kubectl create -f pods/secure-monolith.yaml
	cat services/monolith.yaml
	kubectl create -f services/monolith.yaml
	service "monolith" created
	gcloud compute firewall-rules create allow-monolith-nodeport \
	--allow=tcp:31000
	gcloud compute instances list
	curl -k https://<EXTERNAL_IP>:31000
	// Adding Labels to Pods
	kubectl get pods -l "app=monolith"
	kubectl label pods secure-monolith 'secure=enabled'
	kubectl get pods secure-monolith --show-labels
	kubectl describe services monolith | grep Endpoints
	gcloud compute instances list
	curl -k https://<EXTERNAL_IP>:31000
	// Deploying Applications with Kubernetes
	cat deployments/auth.yaml
	kubectl create -f deployments/auth.yaml
	kubectl create -f services/auth.yaml
	kubectl create -f deployments/hello.yaml
	kubectl create -f services/hello.yaml
	kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
	kubectl create -f deployments/frontend.yaml
	kubectl create -f services/frontend.yaml
	kubectl get services frontend
	curl -k https://<EXTERNAL-IP>
  
  
  