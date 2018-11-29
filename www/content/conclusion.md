---
title: Conclusion
weight: 11
menu: true
---

Congratulations! 🎉 

You completed the workshop for Kubernetes. You have learned all the basic concepts for running applications in Kubernetes.
There's a lot more like _Cronjobs_, _Serverless_, Persistence, etc. but we leave those to next time.

We still have one job to do, clean up our 💩!

And because at the beginning, everyone created own _Namespace_, all we need to do is delete the _Namespace_
```shell
kubectl delete namespace $USER-test

  namespace "xxxx-test" deleted
```

> ACTION: Windows users, you need to replace the `$USER` with your firstname
