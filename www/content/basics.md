---
title: Basics
weight: 3
menu: true
---

## basics of kubectl
`kubectl` is a command line tool to manage Kubernetes. There's is various other open source CLI and GUI tools to do the same and more.

Usually you need few basic commands:

- `kubectl get <resource type>` - List all the _resources_ of given _type_
- `kubectl describe <resource type> <name>` - Show details about the resource
- `kubectl apply -f <some file>` - Create all _resources_ defined in the _file_
- `kubectl delete <resource type> <name>` - Delete _resource_ by _name_
- `kubectl logs <pod name>` - View _Pod_ log output
- `kubectl run <name> --image <image> <cmd>` - Start container and run cmd in it
- `kubectl exec -i -t <name> <cmd>` - Run command in running container

## Few kubectl examples
```shell
# List all pods in the namespace
kubectl get pods

# Show details about Pod
kubectl describe pod hello-world-app-746bb754dd-7z6cq

# Follow Pod log output
kubectl logs hello-world-app-746bb754dd-7z6cq -f
```

## Debugging in Kubernetes

Sometimes you want to "go in" to the cluster with your terminal to debug, test your application endpoint with curl, etc.

With `kubectl run` command you can easily create temporary, throw away, container in the cluster and move your terminal session in it for debugging.

Run `tutum/curl` image the in cluster and move your terminal session into it
```shell
kubectl run -i -t --rm --restart=Never temp --image tutum/curl /bin/bash
# ... wait a moment ...

  If you don't see a command prompt, try pressing enter.

  root@temp:/#
```

> {{% fontawesome info-circle %}} _Leave this open in separate terminal window, we will need this later_

## Resource types
There are multiple built-in resource types, and you can [create more with custom resource types](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions), but usually, a few basic resources are enough to get started.

- `Pod` - One or more containers what are deployed and managed together
- `Deployment` - Manage _Pod_ replica set and do rolling a upgrade of _Pods_
- `Service` - Group one or more _Pods_ together to be a single _Service_
- `Ingress` - Exposes _Service_ to public access
- `ConfigMap` - For application configuration
- `Secret` - To store passwords, etc.

There are even more commands, check `kubectl --help` for more info.

## Manifests
You can describe all Kubernetes _resources_ in YAML format and those are called "manifests". Storing the definitions in a file is good because then you can store them in version control, publish on the internet etc.

Here's example of Pod in yaml format:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: hello-world-app
    image: polarsquad/hello-world-app:master
    imagePullPolicy: Always
    ports:
    - containerPort: 3000
      protocol: TCP
```

We'll explain this with details in the [next section about _Pods_ »]({{< ref "pods.md" >}})