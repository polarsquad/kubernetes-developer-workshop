---
title: Pods
weight: 4
menu: true
---

A _Pod_ is a group of one or more tightly related containers that will run together on the same worker node and in the same Linux namespace(s).
Each _Pod_ has its own IP, Hostname, processes etc. running a single application and/or additional supporting processes, each running in its own container.
_Pods_ are spread out on different worker nodes.

Here's example _Pod_ manifest file
```yaml
# pod.yml
apiVersion: v1
kind: Pod
metadata:
  # The unique name in the namespace for the Pod
  name: hello-world-app
  labels:
    name: hello-world-app
spec:
  # One or more containers what get deployed and managed together
  containers:
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
      # Set some environment variables for the process
      env:
        - name: MY_ENV
          value: foobar
```

For more details what options you have in the `spec`, check [Kubernetes documentation](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.12/#podspec-v1-core).

Save that to `pod.yml` file and you can create new _Pod_ with command
```shell
kubectl apply -f pod.yml

  pod/hello-world-app created
```

After you have created the _Pod_, you can list pods, and see the newly created _Pod_ running
```shell
kubectl get pods -o wide

  NAME               READY   STATUS    RESTARTS   AGE   IP            NODE
  hello-world-app    1/1     Running   0          7m    100.96.5.11   ip-172-20-80-5.eu-west-1.compute.internal
```

And if you still have the [debugging container running]({{< relref "basics.md#debugging-in-kubernetes" >}}), you can call the _Pod_ internal IP

```shell
curl http://<pod ip>:3000/api/version

  {"commit":"2c93ffa5d271f595208d484fa273d718084f40ea","tag":"unknown", ....}
```

🎉 **_YAY!_ Our application is running and we can communicate with it!**

### Viewing _Pod_ logs

Quite commonly you want to view application log output, to analyse what is happening, debug some problem, etc.

Kubernetes makes accessing logs super easy. You can view _Pod_ stdout with `kubectl logs` command
```shell
kubectl logs hello-world-app

# You should see the nodejs server started log line
```

### Deleting _Pod_
To stop the containers, you just delete the _Pod_ and Kubernetes take care of removing whole application
```shell
kubectl delete pod hello-world-app

  pod "hello-world-app" deleted
```

But we want more than just running single containers, we want replication, we want rolling updates. [And _Deployment_ resource provides all that »]({{< ref "deployment.md" >}})