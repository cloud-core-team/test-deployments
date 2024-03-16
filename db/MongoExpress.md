# How to install test-instance of MongoExpress (minikube)

- [ConfigMap](#configmap)
- [Deployment](#deployment)
- [Service](#service-nodeport)
- [Get URL to UI](#get-url-to-ui)

## ConfigMap

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongodb-configmap
data:
  database_url: mongodb
```

## Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-express
  labels:
    app: mongo-express
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo-express
  template:
    metadata:
      labels:
        app: mongo-express
    spec:
      containers:
        - name: mongo-express
          image: mongo-express
          ports:
            - containerPort: 8081
          env:
            - name: ME_CONFIG_MONGODB_ADMINUSERNAME
              valueFrom:
                secretKeyRef:
                  name: mongodb-secret
                  key: mongo-root-username
            - name: ME_CONFIG_MONGODB_ADMINPASSWORD
              valueFrom:
                secretKeyRef:
                  name: mongodb-secret
                  key: mongo-root-password
            - name: ME_CONFIG_MONGODB_SERVER
              valueFrom:
                configMapKeyRef:
                  name: mongodb-configmap
                  key: database_url
```

## Service (NodePort)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mongo-express
  labels:
    app: mongo-express
spec:
  type: NodePort
  ports:
    - port: 8081
  selector:
    app: mongo-express
```

## Get URL to UI

```bash
minikube service mongo-express --url
```
