# How to install test-instance of Apache Kafka (minikube)

- [Deployment](#deployment)
- [Service](#service-nodeport)
- [Get URL to UI](#get-url-to-ui)

## Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: kafka-broker
  name: kafka-broker
  namespace: kafka
spec:
  replicas: 1
  selector:
    matchLabels:
      app: kafka-broker
  template:
    metadata:
      labels:
        app: kafka-broker
    spec:
      hostname: kafka-broker
      containers:
        - env:
            - name: KAFKA_BROKER_ID
              value: "1"
            - name: KAFKA_ZOOKEEPER_CONNECT
              value: zookeeper-service:2181
            - name: KAFKA_LISTENERS
              value: PLAINTEXT://:9092
            - name: KAFKA_ADVERTISED_LISTENERS
              value: PLAINTEXT://kafka-broker:9092
          image: wurstmeister/kafka
          imagePullPolicy: IfNotPresent
          name: kafka-broker
          ports:
            - containerPort: 9092
```

## Service (NodePort)

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: kafka-broker
  name: kafka-broker
  namespace: kafka
spec:
  ports:
    - port: 9092
  selector:
    app: kafka-broker
```

## Get URL to UI

```bash
minikube service kafka --url
```
