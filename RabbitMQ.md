# How to install test-instance of RabbitMQ (minikube)

- [Deployment](#deployment)
- [Service](#service-nodeport)
- [Get URL to UI](#get-url-to-ui)

## Deployment

```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
  name: rabbitmq
spec:
  replicas: 1
  selector:
    matchLabels:
      app: rabbitmq
  template:
    metadata:
      labels:
        app: rabbitmq
    spec:
      containers:
        - name: rabbitmq
          image: rabbitmq:3-management
          ports:
            - containerPort: 5672
              protocol: TCP
            - containerPort: 4369
              protocol: TCP
            - containerPort: 15672
              protocol: TCP
```

## Service (NodePort)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: rabbitmq
  labels:
    app: rabbitmq
spec:
  type: NodePort
  ports:
    - port: 15672
  selector:
    app: rabbitmq
```

## Get URL to UI

```bash
minikube service rabbitmq --url
```
