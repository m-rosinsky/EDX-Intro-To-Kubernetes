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
