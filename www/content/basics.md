---
title: Basics
weight: 3
menu: true
---

## Resource types
There's multiple built in resource types, and you can [create more with custom resource types](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions), but usually few basic resources are enough to get started.

- `Pod` - One or more containers what are deployed and managed together
- `Deployment` - Manage _Pod_ replica set and do rolling upgrade of _Pods_
- `Service` - Group one or more _Pods_ together to be a single _Service_
- `Ingress` - Exposes _Service_ to public access

There's even more commands, check `kubectl --help` for more info.

## basics of kubectl
`kubectl` is command line tool to manage Kubernetes. There's is various other open source CLI and GUI tools to do the same and more.

Usually you need few basic commands:

- `kubectl get <resource type>` - List all the _resources_ of given _type_
- `kubectl describe <resource type> <name>` - Show details about the resource by _name_
- `kubectl apply -f <some file>` - Create all _resources_ defined in the _file_ (more about this later)
- `kubectl delete <resource type> <name>` - Delete _resource_ by _name_
- `kubectl logs <pod name>` - View _Pod_ log output

## Few kubectl examples
```shell
# List all pods in all namespaces
kubectl get pods --all-namespaces

# Show details about Pod
kubectl describe pod hello-world-app-746bb754dd-7z6cq

# Follow Pod log output
kubectl logs hello-world-app-746bb754dd-7z6cq -f
```