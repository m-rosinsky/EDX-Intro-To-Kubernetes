# Chapter 2 - Container Orchestration

## What Are Containers?

<b>Containers</b> are an application-centric method to deliver high-performing, scalable applications on any infrastructure of your choice.

They provide portable, isolated environments to deliver microservices.

A <b>container image</b> bundles the application along with its runtime, libraries, and dependencies.

## What is Container Orchestration?

<b>Container Orchestrators</b> are tools which group systems together to form clusters where containers' deployment and management is automated at scale while meeting the following requirements:

- Fault-tolerance
- On-demand scalability
- Optimal resource usage
- Auto-discovery to automatically discover and communicate with each other
- Accessibility from the outside world
- Seamless updates/rollbacks without any downtime

## Container Orchestrators

- Amazon Elastic Container Service (ECS)
    - Provided by AWS to run containers at scale on its infrastructure
- Azure Container Instances (ACI)
    - Basic container orchestration service by Azure
- Azure Service Fabric
    - Open source container orchestrator provided by Azure
- Kubernetes
    - Open source orchestration tool, originally started by Google, today part of the CNCF project
- Marathon
    - Framework to run containers at scale on Apache Mesos and DC/OS
- Nomad
    - Hashicorp orchestration tool
- Docker Swarm
    - Container orchestrator provided by Docker Inc
    - Part of Docker Engine

## Why Use Orchestrators?

Most orchestrators can:

- Group hosts together while creating a cluster
    - Leverages benefits of distributed systems
- Schedule containers to run on hosts based on resource availability
- Enable containers in a cluster to communicate with each other
- Bind containers and storage resources
- Manage and optimize resource usage
- Bind to load balancing constructs
- Allow for policies to secure access to applications running inside containers
