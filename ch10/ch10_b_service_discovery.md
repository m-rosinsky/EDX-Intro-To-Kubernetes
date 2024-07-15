## Service Discovery

Two methods for discovering Services:

1. Environment Variables
2. DNS

### Environment Variables

As soon as a Pod starts on a node, the `kubelet` daemon adds a set of environment variables for all active services.

Pods will not have these variables set for Services which are created after the Pods are created.

### DNS

Kubernetes add-on for DNS creates a DNS record for each Service.

The format is: `my-svc.my-namespace.svc.cluster.local`.

This is the most common, and recommended solution.

From a client application Pod, we can run the following command:

```
$ kubectl exec client-app-pod-name -c client-container-name -- /bin/sh -c curl -s frontend-svc:80
```

This allows for the cluster internal name resolution and the `kube-proxy` to guide the client request to a frontend Pod.