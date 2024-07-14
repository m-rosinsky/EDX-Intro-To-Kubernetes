# Chapter 9 - Authentication, Authorization, Admission Control

## AAA Overview

Every access to the cluster API server goes through the following access control stages:

- Authentication
    - Authenticate a user based on credentials provided as part of API requests
- Authorization
    - Authorizes the API requests submitted by the authenticated user
- Admission Control
    - Software modules that validate and/or modify user requests
