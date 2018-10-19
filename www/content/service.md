---
title: Service
weight: 5
menu: true
---

To communicate with the Pods, internally in Kubernetes, or later expose it for public access, you need to somehow specify what pods belong together. And that you can do with _Service_. Service takes `selector` what select _Pods_ with the labels.

Here's example _Service_ manifest
```yaml
apiVersion: v1
kind: Service
metadata:
  name: hello-world-app
  labels:
    app: hello-world-app
spec:
  ports:
    - name: http
      port: 80
      protocol: TCP
      targetPort: 3000
  selector:
    app: hello-world-app
```

When you have service created, you can use `hello-world-app` as a hostname, and it gets routed to the target _Pods_.
For example, `http://hello-world-app` HTTP request gets routed to one of the target _Pods_ port 3000 with round-robin balancing.