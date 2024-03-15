# How to install test-instance of Cassandra (minikube)

- [Deployment](#deployment)
- [Service](#service-nodeport)
- [Get URL to UI](#get-url-to-ui)

## Deployment

```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
  name: akhq
spec:
  replicas: 1
  selector:
    matchLabels:
      app: akhq
  template:
    metadata:
      labels:
        app: akhq
    spec:
      containers:
        - name: akhq
          image: tchiotludo/akhq
          env:
            - name: AKHQ_CONFIGURATION
              value: |
                akhq:
                  connections:
                    docker-kafka-server:
                      properties:
                        bootstrap.servers: "kafka-service:9092"
                      schema-registry:
                        url: "http://schema-registry:8085"
                      connect:
                        - name: "connect"
                          url: "http://connect:8083"
          ports:
            - containerPort: 8080
              protocol: TCP
```

## Service (NodePort)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: akhq
  labels:
    app: akhq
spec:
  type: NodePort
  ports:
    - port: 8080
  selector:
    app: akhq
```

## Get URL to UI

```bash
minikube service akhq --url
```
