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

### Demo

See what ACs are enabled by default in the API server:

```
$ kubectl -n kube-system describe pod kube-apiserver-minikube | grep -i admission
```

Start a new Pod without the image pull policy set to `IfNotPresent`:

```
$ kubectl run admitted --image=nginx --image-pull-policy=IfNotPresent
```

Verify the Pod's image pull policy:

```
$ kubectl get pod admitted -o yaml | grep -i imagepull
```

Enable the AC:

```
$ minikube ssh

$ sudo vi /etc/kubernetes/manifests/kube-apiserver.yaml
```

Add `AlwaysPullImages` to pull policy line.