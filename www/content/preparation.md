---
title: Preparation
weight: 2
menu: true
---

## Install CLI
First step is to install `kubectl` - the Kubernetes CLI.

- Homebrew on Mac: `brew install kubernetes-cli`
- Snap on Ubuntu: `sudo snap install kubectl --classic`
- Windows and other: [https://kubernetes.io/docs/tasks/tools/install-kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl)

Ensure that `kubectl` is in your path
```shell
kubectl version

  Client Version: version.Info{Major:"1", Minor:"12", GitVersion:"v1.12.0", GitCommit:"0ed33881dc4355495f623c6f22e7dd0b7632b7c0", GitTreeState:"clean", BuildDate:"2018-09-28T15:20:58Z", GoVersion:"go1.11", Compiler:"gc", Platform:"darwin/amd64"}
  The connection to the server localhost:8080 was refused - did you specify the right host or port?
```
> NOTE: You might get connection error to localhost, ignore that for now

### (optional) Install kubectx
[kubectx](https://github.com/ahmetb/kubectx) provides handy `kubectx` and `kubens` commandline tools for switching contexts and namespaces
- Homebrew on Mac: `brew install kubectx`
- Other: [https://github.com/ahmetb/kubectx#installation](https://github.com/ahmetb/kubectx#installation)

## Setup config

To connect to our temporary Kubernetes cluster, you need to add a cluster, context, and credentials.

```shell
# Create cluster config
kubectl config set-cluster reaktor-workshop --insecure-skip-tls-verify=true --server=https://reaktor-workshop-master.polarsquad.com
# Create authentication config
kubectl config set-credentials reaktor-workshop --username=admin --password=<PASSWORD HERE>
# Create context
kubectl config set-context reaktor-workshop --user=reaktor-workshop --cluster reaktor-workshop
kubectl config use-context reaktor-workshop
```
> **ACTION: Ask the password from the workshop organiser!**

## Create namespace

Pods in Kubernetes are always in some namespace. You can see all the namespaces with `kubectl`:
```shell
kubectl get namespaces

  NAME          STATUS   AGE
  default       Active   8h
  kube-public   Active   8h
  kube-system   Active   8h
```

By default, Kubernetes uses `default` namespace, but it's best practice to use separate namespaces for different things because otherwise the namespace gets full of pods and is hard to manage and also, later on, you can control access by the namespaces.

```shell
# Create new namespace
kubectl create namespace $USER-test
# By default, use the new namespace
kubectl config set-context --current --namespace=$USER-test
```

Now you should see your new namespace and there should not be any pods running in the namespace

```shell
kubectl get namespaces

  NAME            STATUS   AGE
  default         Active   8h
  ernoaapa-test   Active   7s
  kube-public     Active   8h
  kube-system     Active   8h
  testing         Active   1h
```

```shell
kubectl get pods

  No resources found.
```
