# Chapter 6 - Minikube

## What is Minikube?

Popular method to run all-in-one or multi-node local clusters directly on local workstations.

Minikube invokes kubeadm to bootstrap the cluster components inside the previously provisioned nodes.

## Verify Minikube with Docker Driver

Start minikube:

```
$ minikube start --driver=docker
```

Check the status:

```
$ minikube status

minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
```

Stop minikube:

```
$ minikube stop

✋  Stopping node "minikube"  ...
🛑  Powering off "minikube" via SSH ...
🛑  1 node stopped.
```

Delete minikube (only use when decomissioning entire cluster):

```
$ minikube delete
```

## Advanced Minikube Features

The ```minikube start``` command selects a driver isolation software and downloads the latest Kubernetes version components.

It provisioned a single VM (or container) named `minikube` to host the default single-node all-in-one cluster.

Once provisioned, it bootstraps the control plane (with `kubeadm`) and installs the latest version of the container runtime (Docker) that will be the running environment for all containerized applications deployed in the cluster.

This command stores the specs of the cluster so we can restart whenever desired. The object that stores these specs is called a <b>profile</b>.

To check the status of all clusters:

```
$ minikube profile
minikube

$ minikube profile list
|----------|-----------|---------|--------------|------|---------|---------|-------|----------------|--------------------|
| Profile  | VM Driver | Runtime |      IP      | Port | Version | Status  | Nodes | Active Profile | Active Kubecontext |
|----------|-----------|---------|--------------|------|---------|---------|-------|----------------|--------------------|
| minikube | docker    | docker  | 192.168.49.2 | 8443 | v1.30.0 | Stopped |     1 | *              |                    |
|----------|-----------|---------|--------------|------|---------|---------|-------|----------------|--------------------|
```

The IP is the _private_ IP address of the cluster's control plane instance, and the secure port that exposes the API server to control plane components, agents, and clients: `8443`.

We can create new profiles for new minikube clusters by specifying options in the `start` command:

```
$ minikube start --kubernetes-version=v1.27.10 --driver=podman --profile minipod
```

```
$ minikube start --nodes=2 --kubernetes-version=v1.28.1 --driver=docker --profile doubledocker
```

```
$ minikube start --driver=virtualbox --nodes=3 --disk-size=10g --cpus=2 --memory=6g --cni=calico --container-runtime=cri-o -p multivbox
```

The `active` marker in the table of the `minikube profile list` command speicifies which cluster we are currently controlling (the active _context_) with the CLI.

This can be switched by:

```
$ minikube profile <name>
```

## More Minikube Commands

Most minikube commands are _profile aware_, meaning that the user is required to explicitly specify the target cluster of the command.

The default `minikube` cluster, however, can be managed implicitly as we have been doing.

```bash
$ minikube stop -p minibox
$ minikube stop # stop the default minikube 
```

Display the current version of minikube:

```
$ minikube version
minikube version: v1.33.0
commit: 86fc9d54fca63f295d8737c8eacdbb7987e89c67
```

Display node IPs for a cluster:

```
$ minikube node list 
minikube        192.168.49.2

$ minikube node list -p minibox
```

Display cluster control plane node's IP addresses or another node's IP with `--node` or `-n`:

```
$ minikube ip
192.168.49.2

$ minikube -p minibox ip -n minibox-m02
192.168.59.102
```

Delete a profile instance:

```
$ minikube delete -p <name>
```