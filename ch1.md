# Chapter 1 - From Monolith to Microservices

## What is the Monolith?

A _monolith_ is a large, single piece of softrware running on a single system which has to satisfy its compute, memory, storage, and networking requirements.

The hardware of such capacity is complex and pricey.

Downtime can be innevitable when patching or upgrading.

## The Modern Microservice

Microservices are distributed components each described by a set of specific characteristics.

They can be deployed individually on separate servers provisioned with fewer resources.

This is aligned with Service-Oriented Architecture (SOA) principles.

Small, independent processes communicate via API calls over a network.

One of the greatest benefits is <b>scalability</b>, with each component being able to be scaled individually.

## Refactoring

Refactoring a monolith into microservice, cloud-friendly architecutre is tricky.

### Big-Bang Approach

- Focusing all efforts with the refactoring of the monolith
- Postponing new development of any new features

### Incremental Approach

- New features are developed and implemented as modern microservices
- Microservices are able to communicate with the monolith through APIs
- Features are refactored out of the monolith slowly

## Challenges

Some legacy systems are easier to just rebuild from the ground up, rather than refactor.

Containers can provide lightweight runtime environments for application modules.

## Success Stories

- https://kubernetes.io/case-studies/

- https://kubernetes.io/case-studies/appdirect/