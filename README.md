# Demo K8S Application

The following repository contains kubernetes templates for deploying into a local Kubernetes cluster.
<br>
This repository is for demo purposes only.
<br>
## Architecture
```
                                                                     ┌─────────────────────┐
┌────────────────────────┐                                           │                     │
│                        │              ┌──────────────────────┐     │                     │
│     Node Port          │              │                      │     │                     │
│      (30303)           ├──────────────►     Nginx-Service    ├─────►    Nginx-Proxy      │
│                        │              │                      │     │                     │
└────────────────────────┘              └──────────────────────┘     │                     │
                                                                     │                     │
                                                    ┌────────────────┴─────────────────────┘
                                                    │
                                                    │
                                                    │
                                                    │
                                                    │
                                                    │                ┌─────────────────────┐
                                                    │                │                     │
                                                    │                │                     │
                                         ┌──────────▼──────────┐     │                     │
                                         │    React-Service    │     │                     │
                                         │                     ├─────►   React-app         │
                                         └─────────────────────┘     │                     │
                                                                     │                     │
                                                                     │                     │
                                                                     └─────────────────────┘
| 
```

## Requirements
Ensure Minikube is installed.
<br>
https://minikube.sigs.k8s.io/docs/start/

Also ensure that `kubectl` tool is installed.

## Linux / MacOS Install

Clone the repository down and ensure that minikube is started up.
<br>
From there, its as easy as applying the namespace first, then deploying the rest of the yaml files.

```bash
git clone https://github.com/antonsmit30/k8s-react-demo-app.git
# start minikube
minikube start
# ensure minikube is running
cd k8s-react-demo-app/
# deploy ns first
kubectl apply -f deployments/k8s-demo-ns.yml
# deploy the rest of the code:
kubectl apply -f deployments/
```

## Working setup

```bash
$ kubectl get all -n k8s-demo
NAME                                        READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-5c9f7c9b-8l8bq         1/1     Running   0          65s
pod/react-app-deployment-7cbcbc5594-gfzp6   1/1     Running   0          65s

NAME                    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
service/app-service     ClusterIP   10.98.91.206   <none>        80/TCP         65s
service/nginx-service   NodePort    10.98.182.13   <none>        80:30303/TCP   65s

NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment       1/1     1            1           65s
deployment.apps/react-app-deployment   1/1     1            1           65s

NAME                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-5c9f7c9b         1         1         1       65s
replicaset.apps/react-app-deployment-7cbcbc5594   1         1         1       65s

```

## To Test

Open a browser to http://minikubeip:<nodeport>
<br>
i.e: http://192.168.99.101:30303/
