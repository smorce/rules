# Authentication Part 2: Authorization and Access Control

This document outlines best practices for authorization, ensuring that authenticated users can only access the resources and perform the actions they are permitted to.

## Core Principles

- **Deny by Default:** The default access control decision for any request must be to deny access. Permissions should be granted explicitly.
- **Principle of Least Privilege:** Users should be granted the minimum level of access required to perform their duties.
- **Enforce on Every Request:** Authorization checks must be performed on every incoming request, regardless of whether it is from a web browser, a mobile app, or an API client.

## Authorization Models

- **Role-Based Access Control (RBAC):** A widely-used model where permissions are associated with roles, and users are assigned to roles.
- **Attribute-Based Access Control (ABAC):** A more fine-grained model where access decisions can be based on attributes of the user, the resource being accessed, and the environment.
- **Relationship-Based Access Control (ReBAC):** An approach where permissions are defined by the relationship between a user and a resource (e.g., a user is the "owner" of a document).

For applications with complex authorization requirements, prefer ABAC or ReBAC over simple RBAC.

## Common Vulnerabilities and Defenses

- **Insecure Direct Object References (IDOR):**
  - **Vulnerability:** Occurs when an application uses a user-supplied identifier to access a resource without verifying that the user is authorized to access that specific resource.
  - **Defense:** Never trust user-supplied identifiers. Always verify that the current user is authorized to access the requested resource. For example, instead of `SELECT * FROM documents WHERE id = ?`, use `SELECT * FROM documents WHERE id = ? AND owner_id = ?`.

- **Mass Assignment:**
  - **Vulnerability:** Arises when a framework automatically binds incoming request parameters to object properties, potentially allowing an attacker to modify sensitive, unintended fields (e.g., an `isAdmin` flag).
  - **Defense:** Use Data Transfer Objects (DTOs) or an allow-list of fields that can be updated by the user. Avoid binding request data directly to database-mapped objects.

## Transaction Authorization (Step-Up Authentication)

- **High-Risk Actions:** For sensitive operations, such as transferring funds or changing permissions, require the user to re-authenticate or provide a second factor.
- **What-You-See-Is-What-You-Sign (WYSIWYS):** Before confirming a sensitive transaction, display the critical details of the transaction to the user for their review.
