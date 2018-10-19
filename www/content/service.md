---
title: Service
weight: 5
menu: true
---

To communicate with the Pods, internally in Kubernetes, or later expose it for public access, you need to somehow specify what _Pods_ belong together. And that you can do with _Service_. _Service_ takes `selector` what select _Pods_ with the labels.

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

Save this yaml to file `service.yml` and run `kubect apply -f service.yml`

When you have _Service_ created, you can use `hello-world-app` as a hostname, and it gets routed to the target _Pods_.
For example, `http://hello-world-app` HTTP request gets routed to one of the target _Pods_ port 3000 with round-robin balancing.

Let's try in the temporary container what we started with `kubectl run ...` command

```shell
curl http://hello-world-app/api/version

  {"commit":"2c93ffa5d271f595208d484fa273d718084f40ea","tag":"unknown"}
```
