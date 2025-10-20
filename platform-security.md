# Platform Security

This document provides a summary of best practices for securing various application platforms, including web services (APIs), mobile apps, and common web development frameworks.

## Web Service (API) Security

- **Transport Security:**
  - Enforce HTTPS for all API endpoints.
  - For internal or high-value services, consider using mutual TLS (mTLS) for stronger authentication.

- **Authentication and Authorization:**
  - Use standard, well-vetted protocols like OAuth 2.0 and OpenID Connect (OIDC).
  - Enforce authorization checks on every endpoint for every request. Deny by default.
  - Scope API keys and access tokens to the minimum necessary permissions.

- **Input Validation:**
  - Validate all incoming data against a strict schema (e.g., OpenAPI, JSON Schema).
  - Set limits on request size, query complexity (for GraphQL), and rate limiting to prevent denial-of-service attacks.

- **Server-Side Request Forgery (SSRF) Prevention:**
  - Never trust user-supplied URLs.
  - Use a strict allow-list of permitted domains or IP addresses for outbound requests.
  - Block requests to internal or loopback IP addresses.

- **GraphQL-Specific Security:**
  - Disable introspection in production environments.
  - Implement query depth and complexity limits.
  - Enforce field-level and object-level authorization.

## Mobile App Security

- **Server-Side Checks:** Perform all sensitive operations, including authentication and authorization, on the server side. Never trust the client.

- **Secure Storage:**
  - Do not store sensitive data (e.g., passwords, API keys) on the device.
  - Use the platform-provided secure storage mechanisms for storing access tokens:
    - **iOS:** Keychain
    - **Android:** Keystore

- **Data Privacy:**
  - Minimize the collection of Personally Identifiable Information (PII).
  - Avoid caching, logging, or taking screenshots of sensitive data.

- **Code Integrity:**
  - Obfuscate the app's binary to make reverse engineering more difficult.
  - Implement runtime checks to detect if the app is running on a rooted or jailbroken device, or if it has been tampered with.

- **Platform-Specific APIs:**
  - **Android:** Use the Play Integrity API to verify app and device integrity.
  - **iOS:** Use the App Attest and DeviceCheck APIs to validate app and device integrity.

## Secure Coding by Framework

- **General Principles:**
  - **Use Built-in Security Features:** Leverage the security features provided by your framework, such as CSRF protection, XSS prevention (e.g., auto-escaping in templates), and secure session management.
  - **Keep Dependencies Updated:** Regularly update the framework and all third-party libraries to patch known vulnerabilities.
  - **Disable Debug Mode in Production:** Never run applications in debug mode in a production environment.

- **Framework-Specific Guidance:**
  - **Django:** Use `SecurityMiddleware`, enable HSTS, and set secure cookie flags.
  - **Laravel:** Use Eloquent's parameterization to prevent SQL injection and Blade's escaping to prevent XSS.
  - **Ruby on Rails:** Avoid dangerous functions like `eval` and `system`. Use `sanitize_sql_like` for LIKE queries.
  - **.NET Core:** Use `[Authorize]` attributes and anti-forgery tokens.
  - **Node.js:** Use the `helmet` library to set secure HTTP headers and avoid `child_process.exec` with untrusted input.
