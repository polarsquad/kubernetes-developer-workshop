---
title: Ingress
weight: 6
menu: true
---

Usually you want to expose some _Service_ to public internet. That you can do with _Ingress_ resource.

Here's example _Ingress_ manifest
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-app
  annotations:
    kubernetes.io/tls-acme: "true"
spec:
  tls:
  - hosts:
    - <your.name>.k8s-demo.polarsquad.com
    secretName: hello-world-app-tls
  rules:
  - host: <your.name>.k8s-demo.polarsquad.com
    http:
      paths:
      - backend:
          serviceName: hello-world-app
          servicePort: 3000
```

> ACTION: Replace the `<your.name>` subdomain, with your name!

Now you should be able to open [https://<your.name>.k8s-demo.polarsquad.com](https://<your.name>.k8s-demo.polarsquad.com) and see web page with Polar Squad logo!
