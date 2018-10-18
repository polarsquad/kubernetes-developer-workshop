---
title: Deployments
weight: 5
menu: true
---

Usually running single _Pod_ is not enough. You want to run multiple replication of it, update Pods gracefully, etc. you want to use _Deployment_ resource.
_Deployment_ is wrapper around the _Pod_ definition, which enables you to define replication and when you make changes to the Pod specification, it will take care of gracefully rolling the update so that there's no any downtime.

Here's example _Deployment_ manifest
```yaml

---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: hello-world-app
spec:
  # How many instances to run
  replicas: 2
  # Under the template, you have all the same options what you have in Pod specification
  template:
    metadata:
      labels:
        app: hello-world-app
    spec:
      containers:
        - name: hello-world-app
          image: polarsquad/hello-world-app:master
          imagePullPolicy: Always
          ports:
            - containerPort: 3000
          readinessProbe:
            httpGet:
              path: /api/version
              port: 3000
          livenessProbe:
            httpGet:
              path: /api/healthy
              port: 3000
          resources:
            requests:
              cpu: 200m
              memory: 300Mi
            limits:
              cpu: 200m
              memory: 300Mi
```