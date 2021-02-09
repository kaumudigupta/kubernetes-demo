# kubernetes-demo
This is the repository which shows how to get started with Kubernetes and has some basic commands and config files that you may need. 

---

### Basic components 
* Pod - A thin wrapper around one or more containers
* Node - The machines in a Kubernetes cluster 
* Service - Provides a fixed IP address to a logical group of pods
* Ingress - An API object that manages external access to the services in a cluster, typically HTTP (DNS)
* ConfigMap - To store the configurtions 
* Secret - To store the access key and secret or username or password
* Volumes - To have an external storage space
* Deployment - Responsible for how to roll out or roll back across versions of your app
* ReplicaSet - Ensures a defined number of pods are always running
* StatefulSet - For stateful apps like DB

---

#### Control plane 
* One or More API Servers: Entry point for REST / kubectl
* etcd: Distributed key/value store
* Controller-manager: Always evaluating current vs desired state
* Scheduler: Schedules pods to worker nodes

#### Data plane 
* Made up of worker nodes
* kubelet: Acts as a conduit between the API server and the node
* kube-proxy: Manages IP translation and routing

---

#### Basic parts of the config.yml file

1. metadata : Has the name & labels
For eg - 
```
metadata:
  name: nginx-deployment
  labels:
    app: nginx
```
2. spec : Has the config specific to the kind i.e. whether we are writing the config file for a deployment or service. 
For eg - 
```
spec:
  replicas: 1
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
        image: nginx:1.16
        ports:
        - containerPort: 8080

```
3. status : Auto generated by the kubernetes. Maintains the acutal and desired state here and updates the state continously. The information comes from etcd. 

---

#### Other useful tags
* Template : In deployment config, it has its own metadata and spec which defines the pod. 
For eg - 
```
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16
        ports:
        - containerPort: 8080
```
* Labels : In deployment config, it is there in metadata of both the deployment and the pods.
```
      labels:
        app: nginx
```
* Ports: In deployment & service config, it is there in spec to mention the port. Service target port should be the container port. <br/>
    * For container - 
    ```
        ports:
        - containerPort: 8080
    ```

    * For service - 
    ```
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
    ```
* Selectors : In service config, it is used to make the connection between service and depolyment or its pods. 
```
  selector:
    app: nginx

```
---

### For setup

* [To download minikube](https://minikube.sigs.k8s.io/docs/start/)

* [To download  docker](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

* [to set different drivers](https://minikube.sigs.k8s.io/docs/drivers/) - I used docker in mine for windows.

---

### create minikube cluster
``` 
minikube start --vm-driver=docker
kubectl get nodes
minikube status 
kubectl version
```
### Delete the minikube cluster
```
minikube delete
minikube status
```

---

### Basic kubectl commands 

#### Create deployment 

``` kubectl create deployment {deployement-name} --image={image-name} ``` <br/>
For eg -  ``` kubectl create deployment nginx-depl --image=nginx ```

Similarly, <br/>
``` kubectl edit deployment {deployment-name} ``` <br/>
For eg - ``` kubectl edit deployment nginx-depl ```

#### Delete deployment 
```
kubectl delete deployment {deployment-name}
```
--- 

#### Create or edit the deployment config.yml
For example - If we try to make it for nginx, we need to follow the below steps - 

``` 
vim nginx-deployment.yaml
kubectl apply -f nginx-deployment.yaml
kubectl get pod
kubectl get deployment
```

#### To delete using config

```
kubectl delete -f nginx-deployment.yaml
```
---

#### Get commands for various components 

```
kubectl get nodes
kubectl get pod
kubectl get deployment
kubectl get replicaset
kubectl get services
```
---

#### Logs for debug

```
kubectl logs {pod-name}
kubectl exec -it {pod-name} -- bin/bash
```
