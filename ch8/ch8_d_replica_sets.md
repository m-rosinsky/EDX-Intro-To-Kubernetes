## ReplicaSets

- Implements replication and healing aspects for pods.
- Supports both equality and set-based selectors

With the help of the ReplicaSet, we can scale the number of Pods running a specific application container image.

This can be accomplished manually, or through the use of an <b>autoscaler</b>.

![ReplicaSets](./imgs/kb_replica_sets.png)

Despite the Pods being identical, they still have unique names and IP addresses.

Here is a sample object manifest `redis-rs.yaml`:

```yaml
apiVersion: v1
kind: ReplicaSet
metadata:
    name: frontend
    labels:
        app: guestbook
        tier: frontend
spec:
    replicas: 3
    selector:
        matchLabels:
            app: guestbook
    template:
        metadata:
            labels:
                app: guestbook
        spec:
            contianers:
            - name: php-redis
              image: gcr.io/google_samples/gb-frontend:v3
```

```
$ kubectl create -f redis-rs.yaml
$ kubectl get replicasets
$ kubectl get rs
$ kubectl scale rs frontend --replicas=4
$ kubectl get rs frontend -o yaml
$ kubectl describe rs frontend
$ kubectl delete rs frontend
```
