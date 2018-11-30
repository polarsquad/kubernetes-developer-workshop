---
title: Communication
weight: 9
menu: true
---

When you start building service in microservice architecture, you need a way to create multiple services and communicate between those internally.

To simulate this, we start [Redis](https://redis.io/) service in our cluster and update our application to use that for persistence.

```yaml
# redis.yml
---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: redis
  labels:
    app: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis
          image: redis
          ports:
            - containerPort: 6379
          resources:
            requests:
              cpu: 100m
              memory: 100Mi

---
apiVersion: v1
kind: Service
metadata:
  name: redis
  labels:
    app: redis
spec:
  ports:
    - name: http
      port: 6379
  selector:
    app: redis
```

Create the Redis _Deployment_ and _Service_
```shell
kubectl apply -f redis.yml
```

Then we need to update our service to use the new Redis service. Our [hello-world-app](https://github.com/polarsquad/hello-world-app) uses Redis to persist data, if you pass `REDIS_URL` environment variable. Let's update the _Deployment_:
```yaml
# deployment.yml
apiVersion: extensions/v1beta1
kind: Deployment
# ...
          env:
            - name: REDIS_URL
              value: redis://redis
# ...
```

And update the _Deployment_
```shell
kubectl apply -f deployment.yml
```

Usually databases requires some password, so let's try to [add _Secret_ to our service »]({{< ref "secrets.md" >}})
