---
title: Pods
weight: 4
menu: true
---

_Pod_ is a group of one or more tightly related containers that will run together on the same worker node and in the same Linux namespace(s).
Each _Pod_ has its own IP, Hostname, processes etc. running a single application and/or additional supporting processes, each running in its own container.
_Pods_ are spread out on different worker nodes.

Here's example _Pod_ manifest with explanations
```yaml
apiVersion: v1
kind: Pod
metadata:
  # The unique name in the namespace for the Pod
  name: hello-world-app
spec:
  containers:
    # One or more containers what get deployed and managed together
    - name: hello-world-app
      # Docker image to run
      image: polarsquad/hello-world-app:master
      imagePullPolicy: Always
      # What ports the container exposes
      ports:
        - containerPort: 3000
      # Describes how Kubernetes knows when the container is ready
      readinessProbe:
        httpGet:
          path: /api/version
          port: 3000
      # Describes how Kubernetes knows when the container is healthy
      livenessProbe:
        httpGet:
          path: /api/healthy
          port: 3000
      resources:
        # Describes how much resources should be reserved for the container
        requests:
          cpu: 200m
          memory: 300Mi
        # Describes limits when the pod should be killed because of using too much resources
        limits:
          cpu: 200m
          memory: 300Mi
```