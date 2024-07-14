## Authorization Demo

1. View the context of the `kubectl` config manifest:

```
$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: DATA+OMITTED
    server: https://172.30.1.2:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: DATA+OMITTED
    client-key-data: DATA+OMITTED
```

Obeserve that there is 1 user, `kubernetes-admin` and one context, `kubernetes`.

2. Create the `lfs158` namespace:

```
$ kubectl create namespace lfs158
namespace/lfs158 created
```

3. Create the `rbac` directory and `cd` into it:

```
$ mkdir rbac
$ cd rbac
```

4. Create a new user `bob` on the workstation and set the password:

```
$ useradd -s /bin/bash bob
$ passwd bob
New password: 
Retype new password: 
passwd: password updated successfully
```

5. Create a private key for the new user with `openssl`, then create a <b>Certificate Signing Request</b>:

```
$ openssl genrsa -out bob.key 2048
Generating RSA private key, 2048 bit long modulus (2 primes)
.............................................................+++++
.......+++++
e is 65537 (0x010001)
```

```
$ openssl req -new -key bob.key -out bob.csr -subj "/CN=bob/O=learner"
$ ls
bob.csr  bob.key
```

6. Create a YAML manifest for a CSR object, and save it with a blank value in the `request` field:

```
$ vim signing-request.yaml
```

```yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: bob-csr
spec:
  groups:
  - system:authenticated
  request: # blank
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - digital signature
  - key encipherment
  - client auth
```

7. `base64` encode the certificate, and remove newlines:

```
$ cat bob.csr | base64 | tr -d '\n','%'
LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1pUQ0NBVTBDQVFBd0lERU1NQW9HQTFVRUF3d0RZbTlpTVJBd0RnWURWUVFLREFkc1pXRnlibVZ5TUlJQgpJakFOQmdrcWhraUc5dzBCQVFFRkFBT0NBUThBTUlJQkNnS0NBUUVBdHpRL01iK2pxUVhaVE1STC83dFZoTFEyCllxZEpoMzl4eGV3RmdXWnVKS0hWV0toeW1RakthSFpxMjlnWFEvNGRBSHhIQ3N2aTNTOWltVWpwNU55ckhzUXIKcmdGaEZXRHJxb3RRcHhOSnNFUmpKS1BFRE1jV1Irb3htc3VYa1hzSm8xbnJJRGlET204Sjcwc05hNlhJeTZEdwp3REdxUlBDZGgwc0hFWWU3MFJQNkp3Z1ArY3BGV0NHUTBtS3lmRGswZ0tOOFRjTlYrRi9MWjBITFBsWkE0Tjh5CjFBZ012VTVacFBmR2o1WDBQYS9ocjJmUTFLbnphd2s2a0xvS0ZDcjBTQnZEd2twOUpBYXc0MVY0bXhyQmhxYmYKWXVIUENTVERscHUxOHVMenYzRCtaSnZ0NTdzbjVGV1ExajFCMDRyWCs1QVB4aTcra0puMGdtOG5pQjdKc3dJRApBUUFCb0FBd0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFCOG1KOTViRG11RDc3STNGVmM1TVIvSjZ1TTZPem14CjFPMTN3M0lXOG5mcm85a1RIVm1EN2tSemtkSTh6WTluRk5UK0NiRjcvQkJjWWJlUVRaUURsbGg1VjhrZDNjak4KMnN4SnJwYmRQN01yS0RWVnFxQjJlZFhIbS9wQkFveTVJdzM2RG52cDh1N1N1ZG5nSCtCWllHd1c1NlVsMzU2VApzaEZzUWJCblJVVGphQ2RrNjNKWm5UTVFUWitnK3licHdlQUk1eGo3MnV4U09EOWYzNzFYcmpmRTJBWkgzMG5HCnNaRTI3YWVTRmwxdGtMMHg4L1pWNDc1Q2RGTDBkRFZHNmNWUExlUGthWHBsNW0xeGpYWkduMS9uNnFueW1iaU0KT0RxeDRTdFpMMmZzaENrb1ArWC9TZ2orUTM5dFNMSE12eVRKWFlPS2xlTmlyQnlKY2tMVnB1TT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
```

8. Paste this key into the `spec.request` field of the manifest:

```
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: bob-csr
spec:
  groups:
  - system:authenticated
  request: LS0tLS1CRUd...1QtLS0tLQo=
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - digital signature
  - key encipherment
  - client auth
```

9. Create the CSR object, then list the requests:

```
$ kubectl create -f signing-request.yaml 
certificatesigningrequest.certificates.k8s.io/bob-csr created
$ kubectl get csr
NAME      AGE   SIGNERNAME                            REQUESTOR          REQUESTEDDURATION   CONDITION
bob-csr   27s   kubernetes.io/kube-apiserver-client   kubernetes-admin   <none>              Pending
```

10. Approve the CSR:

```
$ kubectl certificate approve bob-csr
certificatesigningrequest.certificates.k8s.io/bob-csr approved
$ kubectl get csr
NAME      AGE   SIGNERNAME                            REQUESTOR          REQUESTEDDURATION   CONDITION
bob-csr   66s   kubernetes.io/kube-apiserver-client   kubernetes-admin   <none>              Approved,Issued
```

11. Extract approved cert from CSR, decode with `base64`, and save as a cert file:

```
$ kubectl get csr bob-csr -o jsonpath='{.status.certificate}' | base64 -d > bob.crt
$ cat bob.crt
-----BEGIN CERTIFICATE-----
MIIDFTCCAf2gAwIBAgIQZyfKEgKlAo0ZoZGRLUN2aTANBgkqhkiG9w0BAQsFADAV
MRMwEQYDVQQDEwprdWJlcm5ldGVzMB4XDTI0MDcxNDIyMzMyM1oXDTI1MDcxNDIy
MzMyM1owIDEQMA4GA1UEChMHbGVhcm5lcjEMMAoGA1UEAxMDYm9iMIIBIjANBgkq
hkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAtzQ/Mb+jqQXZTMRL/7tVhLQ2YqdJh39x
xewFgWZuJKHVWKhymQjKaHZq29gXQ/4dAHxHCsvi3S9imUjp5NyrHsQrrgFhFWDr
qotQpxNJsERjJKPEDMcWR+oxmsuXkXsJo1nrIDiDOm8J70sNa6XIy6DwwDGqRPCd
h0sHEYe70RP6JwgP+cpFWCGQ0mKyfDk0gKN8TcNV+F/LZ0HLPlZA4N8y1AgMvU5Z
pPfGj5X0Pa/hr2fQ1Knzawk6kLoKFCr0SBvDwkp9JAaw41V4mxrBhqbfYuHPCSTD
lpu18uLzv3D+ZJvt57sn5FWQ1j1B04rX+5APxi7+kJn0gm8niB7JswIDAQABo1Yw
VDAOBgNVHQ8BAf8EBAMCBaAwEwYDVR0lBAwwCgYIKwYBBQUHAwIwDAYDVR0TAQH/
BAIwADAfBgNVHSMEGDAWgBTku/EA2b1daY9EckuXGWzhEtY0ODANBgkqhkiG9w0B
AQsFAAOCAQEAe8fr/KafFaSNJXQhAsOfvUKykHQi0hVhwDoYj3F/kcyprLvMFzAP
05jcETK8rOwYC0+JVmazyqZtbrEBPm+fKNBtEAaQoVXwVnRXms6DV4bHIuLBD9YG
JEPbO05Nr4hea+Jb+Gx59Y/ptgkttvGHo10zOs9hz8A3usvsn/KMeQRLtA1Bb8bn
lfB+aiNnArJesei786w3qL6R3b2NgvFxNvQ9mNXGxPYuPQcoqVGQedbeEtZaocTU
I5SCButKfoQGaLkm19tKXz7HuNoOcs0mPoyR0v3p+m7mYymLcp3/Z/zhtXih+d1a
LRSCdo0Fjy1zRdvK5idZ+sJOrbAHnJ3njA==
-----END CERTIFICATE-----
```

12. Configure `kubectl` manifest with user `bob`'s credentials by assigning his key and cert:

```
$ kubectl config set-credentials bob --client-certificate=bob.crt --client-key=bob.key
User "bob" set.
```

13. Create a new `context` entry in the manifest for `bob`, associated with the `lfs148` namespace:

```
$ kubectl config set-context bob-context --cluster=kubernetes --namespace=lfs158 --user=bob
Context "bob-context" created.
```

14. Within the default context, create a new deployment in the `lfs158` namespace:

```
$ kubectl --namespace lfs158 create deployment nginx --image=nginx:alpine
deployment.apps/nginx created
```

15. Attempt to list pods from the `bob-context` context. This fails since `bob` has no permissions configured for the `bob-context`:

```
$ kubectl --context=bob-context get pods
Error from server (Forbidden): pods is forbidden: User "bob" cannot list resource "pods" in API group "" in the namespace "lfs158"
```

16. Create a YAML manifest for a `pod-reader` role, which allows only `get`, `watch`, `list` in the `lfs158` namespace against `pod` resources:

```
$ vi role.yaml
```

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: lfs158
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

```
$ kubectl create -f role.yaml 
role.rbac.authorization.k8s.io/pod-reader created
$ kubectl --namespace lfs158 get roles
NAME         CREATED AT
pod-reader   2024-07-14T22:48:10Z
```

17. Create manifest for a `rolebinding` object, which assigns the permissions of the `pod-reader` role to user `bob`:

```
$ vi rolebinding.yaml
```

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

```
$ kubectl create -f rolebinding.yaml 
rolebinding.rbac.authorization.k8s.io/pod-read-access created
$ kubectl --namespace lfs158 get rolebindings.rbac.authorization.k8s.io -o wide
NAME              ROLE              AGE     USERS   GROUPS   SERVICEACCOUNTS
pod-read-access   Role/pod-reader   2m44s   bob
```

18. Now `bob` can get pods:

```
$ kubectl --context=bob-context get pods
NAME                     READY   STATUS    RESTARTS   AGE
nginx-6f564d4fd9-vm484   1/1     Running   0          10m
```