# Namespace

- kubectl get namespace
- kubectl create namespace my-namespace

What resource can be namespace

- kubectl api-resources --namespaced=false
- kubectl api-resources --namespaced=true


4 namespaces per default

- default = resource you create
- kube-node-lease = heartbeats of nodes
- kube-public = configmap / cluster information
- kube-system = kube process

Why use namespace

- complex application in default namespace
- resource group (db, elk)
- conflicts team (same deployment name are override)
- resource sharing (staging, development) -> (db, elk)
- blue / green deployment (production, new production)
- access and resource limit on namespace

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-configmap
  namespace: my-namespace
data:
  db_url: mysql-service.database
```

- specific namespace
- ```mysql-service.database``` it's database namespace 