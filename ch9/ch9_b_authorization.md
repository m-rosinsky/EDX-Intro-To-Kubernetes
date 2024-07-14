## Authorization

After successful authentcation, the API server validates the requests and either allows or denies them.

Request attributes include:

- User
- Group
- Resource
- Namespace
- API group
- etc.

Some Authorization modules include:

### Node authorization

- authorizes kubelet's read operations for services, endpoints, nodes
- write operations for nodes, pods, and events

### Attribute-Based Access Control (ABAC)

- To enable ABAC mode, start server with `--authorization-mode=ABAC` and `--authorization-policy-file=<name>.json`
- In this example, user `bob` can only read Pods in Namespace `lfs158`:

```json
{
    "apiVersion": "abac.authorization.kubernetes.io/v1beta1",
    "kind": "Policy",
    "spec": {
        "user": "bob",
        "namespace": "lfs158",
        "resource": "pods",
        "readonly": true
    }
}
```

### Webhook

- Kubernetes can request authorization decisions to be made by third-party services

### Role-Based Access Control (RBAC)

- Regulate access based on the roles of individual users
- Restrict resource access by specific operations (<b>verbs</b>):
    - `create`
    - `get`
    - `update`
    - `patch`
    - etc.

- In RBAC, there are two kinds of roles:
    - Role: Access to specific resources within a specific Namespace
    - ClusterRole: Same permissions as Role, but cluster wide

- An exmaple of Role:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: lfs158
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

This defines a `pod-reader` role which has access readonly access to the Pods of the `lfs158` Namespace.

- An example of ClusterRole:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-admin
rules:
- apiGroups:
  - '*' # All API groups
  resources:
  - '*' # All resources
  verbs:
  - '*' # All verbs
  nonResourceURLs:
    - '*' # All non-resource URLs, such as "/healthz"
    verbs:
    - '*' # All operations
```

Once the role is created, we bind it to users with a <b>RoleBinding</b> object.

Two kinds of bindings:

- RoleBinding
- ClusterRoleBinding

An example RoleBinding:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-read-access
  namespace: lfs158
subjects:
- kind: User
  name: bob
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

To enable RBAC mode, the API server is start with the `--authorization-mode=RBAC` option.
