# Chapter 4 - Kubernetes Architecture

## Kubernetes Architecture

Kubernetes is a cluster of compute systems categorized by their distinct roles:

- One or more <b>control plane</b> nodes
- One or more <b>worker</b> nodes

![Kubernetes Architecture](./imgs/kb_arch.png)

## Control Plane Node Overview

The <b>control plane node</b> provides a running environment for the <b>control plane agents</b>.

The agents are responsible for managing the state of a Kubernetes cluster, and is the brain behind all operations inside the cluster.

### User Interaction

Users interact with the control plane by the following methods:

- CLI tool
- Web UI Dashboard
- API

### Control Plane Fault Tolerance

It is important to keep the control plane running at all costs.

Control plane node replicas can be added to the cluster, configured in <b>High-Availability</b> (HA) mode.

### Cluster State Persistence

All cluster config data is saved to a distributed key-value store which only holds cluster state related data.

No client workload generated data is stored here.

The key-value store may be configured:

- On the control plane node (_stacked topology_)
- On its dedicated host (_external topology_)

## Control Plane Node Components

The control plane runs the following essential components and agents:

- API Server
- Scheduler
- Controller Managers
- Key-Value Data Store

It also runs:

- Container Runtime
- Node Agent (_kubelet_)
- Proxy (_kube-proxy_)
- Optional observability addons such as:
    - Dashboards
    - Cluster-level monitoring
    - Logging

## Control Plane Node Components: API Server

All administrative tasks are coordinated by the <b>kube-apiserver</b>.

This API server intercepts RESTful calls from users, admins, devs, operators, and external agents.

Processing generally follows:
1. Read cluster's current state from key-value store
2. Execute call
3. Save resulting state to key-value store

The API server is the only component to talk to the key-value store.

## Control Plane Node Components: Scheduler