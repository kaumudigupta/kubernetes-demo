## Mongo Demo APP

This is basically using the mongodb and mongo-express here. 

* mongodb.yaml : Contains the deployment and service for the mongo db. It also uses the secret. 
* mongo-secret.yaml : Contains the username and password in the base64 format. 
* mongo-configmap.yaml : Contains the DB url which is used by the mongo express deployment. 
* mongo-express.yaml : Contains the deployment and service of the mongo express. This uses the `LoadBalancer` type as it is an external service. 
---

### To do the demo, please use this following commands - 

#### kubectl apply commands in order
    
    kubectl apply -f mongo-secret.yaml
    kubectl apply -f mongodb.yaml
    kubectl apply -f mongo-configmap.yaml 
    kubectl apply -f mongo-express.yaml

---
#### kubectl get commands

    kubectl get pod
    kubectl get pod --watch
    kubectl get pod -o wide
    kubectl get service
    kubectl get secret
    kubectl get all | grep mongodb

---
#### kubectl debugging commands

    kubectl describe pod mongodb-deployment-xxxxxx
    kubectl describe service mongodb-service
    kubectl logs mongo-express-xxxxxx

---
#### give a URL to external service in minikube

    minikube service mongo-express-service
    
---
