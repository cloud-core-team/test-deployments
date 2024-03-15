# How to install test-instance of Zookeeper (minikube)

- [Deployment](#deployment)
- [Service](#service-nodeport)

## Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: zookeeper
  name: zookeeper
  namespace: kafka
spec:
  replicas: 1
  selector:
    matchLabels:
      app: zookeeper
  template:
    metadata:
      labels:
        app: zookeeper
    spec:
      containers:
        - image: wurstmeister/zookeeper
          imagePullPolicy: IfNotPresent
          name: zookeeper
          ports:
            - containerPort: 2181
```

## Service (NodePort)

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: zookeeper-service
  name: zookeeper-service
  namespace: kafka
spec:
  type: NodePort
  ports:
    - name: zookeeper-port
      port: 2181
      targetPort: 2181
  selector:
    app: zookeeper
```