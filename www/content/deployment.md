---
title: Deployments
weight: 5
menu: true
---

Usually running single _Pod_ is not enough. You want to run multiple replications of it, update _Pods_ gracefully, etc. you want to use _Deployment_ resource.
_Deployment_ is a wrapper around the _Pod_ definition, which enables you to define replication count and when you make changes to the Pod specification, it will take care of gracefully rolling the update so that there's no any downtime.

Here's example _Deployment_ manifest file
```yaml
# deployment.yml
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
          env:
            # Pass some metadata to the application through environment variables
            - name: NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
            - name: POD_UID
              valueFrom:
                fieldRef:
                  fieldPath: metadata.uid
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_IP
              valueFrom:
                fieldRef:
                  fieldPath: status.podIP
            - name: HOST_IP
              valueFrom:
                fieldRef:
                  fieldPath: status.hostIP
```

Save that to `deployment.yml` file and you can create new _Deployment_ with command
```shell
kubectl apply -f deployment.yml

  deployment/hello-world-app created
```

Now we can see that the _Deployment_ start to create _Pods_ to reach the desired `replicas`.

```shell
kubectl get deployments

  NAME              DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
  hello-world-app   2         2         2            1           2d
```

😯 **_COOL!_ We can see that it's spinning up the _Pods_ for us**

Let's check the _Pods_ what it creates
```shell
kubectl get pods

  NAME                               READY   STATUS              RESTARTS   AGE
  hello-world-app-5b7bb56598-rzbtn   1/1     Running             0          2d
  hello-world-app-5b7bb56598-szhj5   0/1     ContainerCreating   0          9s
```

Try out changing the `replicas` number in the `deployment.yml` and again run the `kubectl apply -f deployment.yml`.

Now we have multiple instances of _Pods_ running but we want to access "one of the _Pods_". [For that we need _Service_ »]({{< ref "service.md" >}})