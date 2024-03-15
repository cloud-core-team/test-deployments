# How to install test-instance of Cassandra (minikube)

- [Deployment](#deployment)
- [Service](#service-nodeport)
- [Get URL to UI](#get-url-to-ui)

## Deployment

```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
  name: cassandra
spec:
  replicas: 1
  selector:
    matchLabels:
      app: cassandra
  template:
    metadata:
      labels:
        app: cassandra
    spec:
      containers:
        - name: cassandra
          image: cassandra:5
          ports:
            - containerPort: 7000
              protocol: TCP
            - containerPort: 9042
              protocol: TCP
```

## Service (NodePort)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: cassandra
  labels:
    app: cassandra
spec:
  type: NodePort
  ports:
    - port: 9042
  selector:
    app: cassandra
```

## Get URL to UI

```bash
minikube service cassandra --url
```
