---
title: Secrets
weight: 9
menu: true
---

Of course all applications need some secrets like database passowrds, API keys, etc.

Kubernetes provides _Secret_ resource to store the secrets and feed them to the application through environment variables when the _Pod_ starts.

Here's example _Secret_ manifest file

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
kubectl get pods

# Copy pod name

kubectl logs <pod name>

# You should see the secret in the output
```
