# Chapter 8 - Kubernetes Building Blocks

## Kubernetes Object Model

Kubernetes manages application lifecycle capabilities through a rich object model, representing different persistent entities in the cluster:

- What containerized applications are running
- The nodes where the containerized applications are deployed
- Application resource consumption
- Policies attached to applications, such as upgrade policies, fault tolerance, ingress/egress, access control, etc.

With each object, we declare the desired state in the `spec` solution.

Kubernetes manages the `status` section for objects, where it records the _actual_ state.

An object definition manifest must include other fields such as:

- The API version (`apiVersion`)
- The object type (`kind`)
- Additional helpful data (`metadata`)

In certain object definitions, `spec` is replaced by `data` and `stringData`.

Example of Kubernetes object types are:

- Nodes
- Namespaces
- Pods
- ReplicaSets
- Deployments
- DaemonSets
- etc.

The API request to create an object must have a `spec` section describing the desired state.

## Nodes

<b>Nodes</b> are virtual identities assigned by Kubernetes to the systems part of the cluster. These can be containers, VMs, bare-metal, etc.

Each node is managed with the help of the `kubelet` and the `kube-proxy`.

Based on their functions, there are two distinct types of nodes:

- Control Plane
- Worker

Node identities are assigned during the cluster bootstrapping process.

For example, Minikube uses `kubeadm` to initialize the control plane during the <b>init</b> phase and grow the cluster by adding worker nodes in the <b>join</b> phase.
