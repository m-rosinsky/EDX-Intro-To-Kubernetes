# Chapter 9 - Authentication, Authorization, Admission Control

## AAA Overview

Every access to the cluster API server goes through the following access control stages:

- Authentication
    - Authenticate a user based on credentials provided as part of API requests
- Authorization
    - Authorizes the API requests submitted by the authenticated user
- Admission Control
    - Software modules that validate and/or modify user requests

## Authentication

Kubernetes supports two kinds of <b>users</b>:

- Normal Users
    - Managed outside of the cluster via independent services like User/Client certs, a file listing usernames/passwords, Google accounts, etc.
- Service Accounts
    - Allows in-cluster processes to communicate with the API server
    - Most are created automatically via the API server, but can also be created manually
    - Accounts are tied to a particular namespace and mount the respective credentials to communicate with API server as secrets

For authentication, Kubernetes uses a series of modules:

- X509 Client Certificates
    - Pass the `--client-ca-file=<name>` option to API server
- Static Token File
    - Passing pre-defined bearer tokens with `--token-auth-file=<name>`
- Bootstrap Tokens
- Service Account Tokens
- OpenID Connect Tokens
    - OAuth2 providers
- Webhook Token Authentication
- Authentication Proxy

Multiple of these can be enabled to allow multiple options for auth.

## Authorization

After successful authentcation, the API server validates the requests and either allows or denies them.

Request attributes include:

- User
- Group
- Resource
- Namespace
- API group
- etc.

Some Authorization modules include:

- Node authorization
    - authorizes kubelet's read operations for services, endpoints, nodes
    - write operations for nodes, pods, and events
- Attribute-Based Access Control (ABAC)
    - To enable ABAC mode, start server with `--authorization-mode=ABAC` and `--authorization-policy-file=<name>.json`
    - In this example, user `bob` can only read Pods in Namespace `lfs158`:

```json
{
    "apiVersion": "abac.authorization.kubernetes.io/v1beta1",
    "kind": "Policy",
    "spec": {
        "user": "bob",
        "namespace": "lfs158",
        "resource": "pods",
        "readonly": true
    }
}
```

- Webhook
    - Kubernetes can request authorization decisions to be made by third-party services
