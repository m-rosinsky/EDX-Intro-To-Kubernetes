## Pods

Pods are the smallest workload object, and the unit of deployment in Kubernetes. It represents a single instance of the application.

Containers in a pod are:

- Scheduled together on the same host with the pod
- Share the same network namespace, meaning they share a single IP address originally assigned to the pod
- Have access to mount the same external storage (volumes) and other dependencies

![Container Pods](./imgs/kb_pods.png)

Pods cannot manage themselves, so controllers are used to handle management.

Controllers include:

- Deployments
- ReplicaSets
- DaemonSets
- Jobs
- etc.

An example of a stand-alone pod object's definition, without a controller:

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx-pod
    labels:
        run: nginx-pod
spec:
    containers:
    - name: nginx-pod
      image: nginx:1.22.1
      ports:
      - containerPort: 80
```

This pod creates a single container running the `nginx:1.22.1` image pulled from a container registry.

The pod's name and labels are used for workload accounting purposes.

This file can be saved as `def-pod.yaml` and loaded into a cluster with `create`:

```
$ kubectl create -f def-pod.yaml
```

We can also simply run the pod defined above without the manifest file:

```
$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80
```

The manifest can be generated from an imperative command as such:

```
$ kubectl run nginx-pod --image=nginx:1.22.1 --port=80 --dry-run=client -o yaml > nginx-pod.yaml
```

They can also be applied in a hot state:

```
$ kubectl apply -f nginx-pod.yaml
```

More helpful pods commands:

```
$ kubectl get pods
$ kubectl get pod nginx-pod -o yaml
$ kubectl get pod nginx-pod -o json
$ kubectl describe pod nginx-pod
$ kubectl delete pod nginx-pod
$ kubectl replace --force -f nginx-pod.yaml
```
