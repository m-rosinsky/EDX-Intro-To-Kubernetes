# Chapter 3 - Kubernetes

## What is Kubernetes?

<b>Kubernetes</b> is an open-source system for automating deployment, scaling, and management of containerized applications.

It is written in Go and licensed under the Apache License, Version 2.0

## Kubernetes Features

- Automatic bin packing
    - Schedules containers based on resource needs and constraints
- Designed for extensibility
    - Clusters can be extended with new custom features without modifying the upstream source code
- Self-healing
    - Automatically replaces containers from failed nodes
- Horizontal scaling
    - Scales applications manually or auto based o CPU or custom metrics utilization
- Service discovery and load balancing
    - Containers receive IP addresses from Kubernetes, which assists in load balancing

## Additional Features

- Automated rollouts and rollbacks
- Secret and config management
- Storage orchestration
- Batch execution
- IPv4/IPv6 dual-stack