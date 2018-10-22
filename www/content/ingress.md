---
title: Ingress
weight: 7
menu: true
---

Usually you want to expose some _Service_ to public Internet. That you can do with _Ingress_ resource.
_Ingress_ resource allows inbound connections to reach the _Services_ running in cluster.

Using _Ingress_ requires that you have configured your cluster to be accessable from Internet and ingress controller to be deployed to the cluster. This is specific to Kubernetes installation. If you need help, contact [Polar Squad](https://polarsquad.com) 😎

Here's example _Ingress_ manifest file
```yaml
# ingress.yml
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
          servicePort: 80
```

> ACTION: Replace the `<your.name>` subdomain, with your name!

Save that to `ingress.yml` file and you can create new _Ingress_ with command
```shell
kubectl apply -f ingress.yml

  ingress/hello-world-app created
```

Now you should be able to open [https://`your.name`.k8s-demo.polarsquad.com](https://your.name.k8s-demo.polarsquad.com) and see web page with Polar Squad logo!

![screenshot](/img/screenshot.png)