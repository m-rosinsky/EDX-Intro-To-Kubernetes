## Daemon Sets

<b>DaemonSets</b> are operators designed to manage node agents.

DaemonSets are distinct from ReplicaSets in that they enforce a single-pod replice to be placed per node on all nodes or on a select subset of nodes.

In contrast, ReplicaSet and Deployment by default have no control over scheduling and placement of multi-pod replicas on the same node.

These are commonly used when we need to collect <b>monitoring</b> data from all Nodes, or to run storage, networking, or proxy daemons on all Nodes.

Examples of DaemonSet operators include:

- The `kube-proxy` agent running as a Pod on every node in the cluster
- The <b>Calico</b> or <b>Cilium</b> networking node agents on every node

Whenever a Node is added to a cluster, a Pod from a given DaemonSet is automatically placed on it.

When a node crashes, the respective DaemonSet operated Pods are garbage collected.

If a DaemonSet is deleted, all Pod replicas it has created are deleted as well.

Node _affinity_, _taints_, and _tolerations_ are used to control which subset of Nodes a DaemonSet places Pods on. For example, we may only want to monitor worker nodes in the cluster.

An example DaemonSet manifest:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-agent
  namespace: default
  labels:
    k8s-app: fluentd-agent
spec:
  selector:
    matchLabels:
      k8s-app: fluentd-agent
  template:
    metadata:
      labels:
        k8s-app: fluentd-agent
    spec:
      containers:
      - name: fluentd
        image: quay.io/fluentd_elasticsearch/fluentd:v4.5.2
```

and created with:

```
$ kubectl create -f def-daemon.yaml
```

```
$ kubectl apply -f fluentd-ds.yaml --record
$ kubectl get daemonsets
$ kubectl get ds -o wide
$ kubectl get ds fluentd-agent -o yaml
$ kubectl get ds fluentd-agent -o json
$ kubectl describe ds fluentd-agent
$ kubectl rollout status ds fluentd-agent
$ kubectl rollout history ds fluentd-agent
$ kubectl rollout history ds fluentd-agent --revision=1
$ kubectl set image ds fluentd-agent fluentd=quay.io/fluentd_elasticsearch/fluentd:v4.6.2 --record
$ kubectl rollout history ds fluentd-agent --revision=2
$ kubectl rollout undo ds fluentd-agent --to-revision=1
$ kubectl get all -l k8s-app=fluentd-agent -o wide
$ kubectl delete ds fluentd-agent
$ kubectl get ds,po -l k8s-app=fluentd-agent
```