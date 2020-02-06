---
title: Configmaps
weight: 11
menu: true
---

You might need to decouple configuration artifacts from image content to keep containerized applications portable.

Kubernetes provides _Configmap_ resource to store the values and feed them to the application through environment variables or files when the _Pod_ starts.


Here's an example _Configmap_ manifest file


```yaml
# configmap.yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config-map
data:
  configmap-key: configmap-value
  my-configmap-key: configmap-value
```
Create the _Configmap_
```shell
kubectl apply -f configmap.yml
```

Next you need to reference the configmap as environment variables

```yaml
# deployment.yml
apiVersion: extensions/v1beta1
kind: Deployment
# ...
          envFrom:
            - configMapRef:
                name: my-config-map
# ...
```

Apply changes in _Deployment_ manifest file
```shell
kubectl apply -f deployment.yml
```

Now if you refresh the webpage, you should see populated configmap key-value pairs.


[Time to wrap up what we learned »]({{< ref "conclusion.md" >}})
