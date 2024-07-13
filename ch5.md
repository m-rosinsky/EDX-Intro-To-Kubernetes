# Chapter 5 - Installing Kubernetes

## Kubernetes Configuration

Kubernetes can be installed using different cluster configs:

- All-in-One Single Node Installation
    - Not recommended for production, but useful for learning
- Single-Control Plane and Multi-Worker Installation
    - Single-control plane node running a stacked etcd instance
    - Multiple worker nodes can be managed by control plane
- Single-Control Plane with Single-Node etcd, and Multi-Worker Installation
    - Single-control plane node with external etcd instance
- Multi-Control Plane and Multi-Worker Installation
    - Multiple control plane nodes configured for HA
    - Each control plane node running stacked etcd
- Multi-Control Plane with Multi-Node etc, and Multi-Worker Installation
    - Multiple control plane nodes configured in HA mode
    - Each control plane node paired with external etcd instance
    - External etcd instances are configured in HA etcd cluster
    - Recommended for production environments

## Infrastructure for Kubernetes Installation

Infra decisions are typically guided by desired environment type.

We need to consider:

- Should we set up Kubernetes on bare metal, public cloud, private, or hybrid cloud?
- Which underlying OS should we use?
- Which CNI should we use?

## Installing Local Learning Clusters

These are installation tools to allow us to deploy single or multi-node clusters on our workstations for learning and development purposes:

- Minikube
    - Single and multi node cluster
    - Recommended for learning environments deployed on a single host
- Kind
    - Multi node clusters deployed in Docker containers acting as Kubernetes nodes
- Docker Desktop
    - Including a local Kubernetes cluster for Docker users
- MicroK8s
    - Local and cloud clusters for developers and production, from Canonical

## Installing Production Clusters with Deployment Tools

- kubeadm
    - Secure and recommended method to bootstrap a multi-node production ready HA cluster
    - On premises or in cloud
    - Does not support provisioning of hosts, which should be provisioned separately
- kubespray
- kops

## Production Clusters from Certified Solutions Providers

- Hosted solutions
    - Amazon Elastic Kubernetes Service (EKS)
    - Azure Kubernetes Service (AKS)
    - Google Kubernetes Engine (GKE)
