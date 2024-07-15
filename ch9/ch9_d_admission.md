## Admission Control

Admission Controllers are used to specify granular access control policies which include:

- Allowing privileged containers
- Checking resource quota
- etc.

These come into effect only after API requests are authenticated and authorized.

ACs fall under 2 categories:

- Validating
- Mutating
- Both

Mutating controllers can modify the requested objects.

To enable, we use the `--enable-admission-plugins` flag, which takes comma delimited ordered list of controller names, such as:

```
--enable-admission-plugins=NamespaceLifecycle,ResourceQuota,PodSecurity,DefaultStorageClass
```

Kubernetes has some of these enabled by default.

ACs can also be implemented through custom plugins, which are developed as extensions and run as admission webhooks.
