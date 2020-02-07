---
title: Secrets
weight: 10
menu: true
---

Of course all applications need some secrets like database passwords, API keys, etc.

Kubernetes provides _Secret_ resource to store the secrets and feed them to the application through environment variables when the _Pod_ starts.

Here's an example _Secret_ manifest file

```yaml
# secret.yml
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
type: Opaque
data:
  # key & base64 encoded value pairs
  my-secret: eW91IGdvdCBpdCE=

```
Create the _Secret_
```shell
kubectl apply -f secret.yml
```

Next you need to map secret to environment variable

```yaml
# deployment.yml
apiVersion: extensions/v1beta1
kind: Deployment
# ...
          envs:
            - name: MY_SECRET
              valueFrom:
                secretKeyRef:
                  name: mysecret
                  key: my-secret
# ...
```

Apply changes in _Deployment_ manifest file
```shell
kubectl apply -f deployment.yml
```

Now if you check the _Pods_ log output, you should see log line showing the secret
```shell
kubectl logs --selector app=hello-world-app

# You should see the secret in the output
```
> Note: Here we used `--selector` so we don't need to copy randomly generated _Pod_ name

Let's add some deployment specific configurations [add _Configmap_ to our service »]({{< ref "configmaps.md" >}})