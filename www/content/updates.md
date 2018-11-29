---
title: Updates
weight: 8
menu: true
---

Congratulations, now you have service running in Kubernetes!

Later on you want to update your service without any downtime. Kubernetes makes this easy for you!
If you update the _Deployment_ and `kubectl apply -f deployment.yml`, Kubernetes start to make rolling upgrade, start and stop _Pods_ to reach the desired state.

Here's updated _Deployment_ manifest file

**note the image tag changed `master`->`jeremy`**
```yaml
# ingress.yml
apiVersion: extensions/v1beta1
kind: Deployment
# ...
    spec:
      containers:
        - name: hello-world-app
          image: polarsquad/hello-world-app:jeremy
# ...
```

Apply the change to the _Deployment_
```shell
kubectl apply -f deployment.yml
```

Now you should be able to open [https://`your.name`.demo-apps.polarsquad.com](https://your.name.demo-apps.polarsquad.com) and see our one eye Jeremy!

![Jeremy screenshot](/img/screenshot-jeremy.png)


The applications are rarely this simple, you depend on to other services.<br>
[Let's create Redis service and test Service-to-Service communication »]({{< ref "communication.md" >}})