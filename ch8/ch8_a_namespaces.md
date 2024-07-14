## Namespaces

If multiple users and teams use the same cluster, it can be partitioned into virtual sub-clusters using <b>Namespaces</b>.

To list all namespaces:

```
$ kubectl get namespaces
```

Four namespaces are generally created out of the box:

- `kube-system`
    - Contains the objects created by the Kubernetes system, mostly the control plane agents
- `kube-public`
    - Unsecured and readable by anyone
    - Used for purposes such as exposing non-sensitive info about the cluster1
- `kube-node-lease`
    - Holds node lease objects used for node heartbeat data.
- `default`
    - Contains the objects and resources created by admins and devs
    - Objects are assigned to it by default unless another Namespace is provided by the user

Good practice is to create additional Namespaces as desired to virtualize the cluster and isolate users, dev teams, applications, or tiers:

```
$ kubectl create namespace <name>
```
