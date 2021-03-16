# Pre-require

- installation minikube

## Kubectl Command

```
kubectl version
kubectl get node
kubectl get deployment
kubectl get service
kubectl get replicaset
kubectl get pod
kubectl get secret
kubectl get all

kubectl describe pod <pod name>
kubectl logs <pod name>
kubectl exec -it <pod name> -- bin/bash

kubectl create deployment <deployment name> --image=<docker image>
kubectl edit deployment <deployment name>
kubectl delete deployment <deployment name>

kubectl apply -f nginx-deployment.yaml 
kubectl delete -f nginx-deployment.yaml 
```

nginx-deployment.yaml 

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: nginx 
spec:
  replicas: 2
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
            - containerPort: 80
```