---
layout: post
title:  "Kubernetes"
date:   2021-01-21 01:27:15 +0200
categories: DevOps
---
In this page you'll find `Kubernetes` commands and other useful information.

<style>
.tablelines table, .tablelines td, .tablelines th {
        border: 1px solid black;
        }
</style>

### Commands

| **Command**  | **What does it do?** |
|:-------------:|:-------------:|
| `kubectl create -f service-definition.yml`             | Create a service from the yml file   |
| `kubectl get services`                                 | List all the services running    |
| `kubectl get services -l app=nginx-app`                | List the service related to the app specified    |
| `kubectl describe svc my-service`                      | Check all the detail of the services    |
| `kubectl delete svc my-service`                        | Delete the services, this deletes the pods as well    |
| `kubectl get pods -o wide`                             | Check the pods and see in which node the pod is scheduled   |
| `kubectl get no -o wide`                               | Check the nodes in k8s cluster   |
| `kubectl taint nodes node-name key=value:taint-effect` | Taint a node so pods can not be put inside it |
| `kubetct taint nodes node1 app=blu:NoSchedule`         | Example of tainting a node  |
| `kubectl describe node kubemaster | grep Taint`        |  |
{: .tablelines}

### Services

---

### NodePort Service

### Example yml file for a NodePort service
```
#Service
#nginx-svc-np.yml
apiVersion: v1
kind: Service
metadata:
	name: my-service
	labels:
		app: nginx-app
spec:
	selector:
		app: nginx-app
	type: NodePort
	port: 
		-nodePort: 30008
		 port: 80
		 targetPort: 80
```

### Deployment

###Example yml file for a Deployment.

```sh
#Service
#nginx-svc-np.yml
apiVersion: apps/v1
kind: Deployment
metadata:
	name: redis-master
	labels:
		app:redis
spec:
	replicas: 1
	selector: 
		matchLabels:
			app: redis
			role: master
			tier: backend
	template:
		metadata:
			labels:
				app: redis
				role: master
				tier: backend
		speck:
			containers:
				-name: master
				 image: <name of the image>
				 resources:
					requests:
						cpu:100m
						memory:100Mi
				 ports:
					-containerPort: 8002
```