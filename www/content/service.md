---
title: Service
weight: 6
menu: true
---

To communicate with the _Pods_, internally in Kubernetes, or later expose it for public access, you need to somehow specify what _Pods_ belong together. And that you can do with _Service_. _Service_ takes `selector` what select _Pods_ by the labels.

Here's example _Service_ manifest file
```yaml
# service.yml
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

Save that to `service.yml` file and you can create new _Service_ with command
```shell
kubectl apply -f service.yml

  service/hello-world-app created
```

When you have _Service_ created, you can use `hello-world-app` as a hostname, and it gets routed to the target _Pods_.
For example, `http://hello-world-app` HTTP request gets routed to one of the target _Pods_ port 3000 with round-robin balancing.

And if you still have the [debugging container running]({{< relref "basics.md#debugging-in-kubernetes" >}}), you test to use the _Service_ name as hostname

```shell
curl http://hello-world-app/api/version

  {"commit":"2c93ffa5d271f595208d484fa273d718084f40ea","tag":"unknown", .... }
```

> {{% fontawesome info-circle %}} _You can use `hostname.namespace` to make request to another namespace! Ask namespace from your colleaque next to you, and try out!_

Now we can connect to _Service_ internally in Kubernetes. But we want to expose it to public Internet, and that [we can do with _Ingress_ »]({{< ref "ingress.md" >}})