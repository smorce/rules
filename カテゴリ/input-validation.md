# Input Validation

This document provides a summary of best practices for input validation, focusing on preventing injection attacks, securing client-side components, and safely handling complex file formats.

## Core Principles

- **Trust Boundary Validation:** Validate all data crossing a trust boundary. Perform validation as early as possible.
- **Positive Validation (Allow-listing):** Define what is allowed, rather than trying to block what is disallowed. This applies to data formats, character sets, and file types.
- **Canonicalization:** Normalize data before validation to prevent evasion techniques (e.g., URL encoding, Unicode).

## Preventing Injection Attacks

- **SQL Injection:**
  - **Primary Defense:** Use parameterized queries (prepared statements). Never build SQL queries by concatenating user input.
  - **Least Privilege:** Application database accounts should have the minimum necessary permissions.

- **OS Command Injection:**
  - **Avoid `exec`:** Prefer using built-in library functions over shelling out to OS commands.
  - **Structured Execution:** If OS commands are unavoidable, use APIs that separate the command from its arguments (e.g., `ProcessBuilder` in Java).

- **LDAP Injection:**
  - **Contextual Escaping:** Apply proper escaping for LDAP Distinguished Names (DNs) and search filters.

- **Cross-Site Scripting (XSS):**
  - **Context-Aware Encoding:** Encode output based on the context in which it is rendered (HTML, HTML attribute, JavaScript, CSS).
  - **Sanitization:** For user-provided HTML, use a well-vetted sanitization library (e.g., DOMPurify) with a strict allow-list of tags and attributes.
  - **Content Security Policy (CSP):** Implement a strict CSP to mitigate the impact of XSS vulnerabilities. Nonce-based or hash-based policies are preferred over domain allow-lists.

## Securing Web Clients

- **Cross-Site Request Forgery (CSRF):**
  - **Synchronizer Tokens:** Use anti-CSRF tokens for all state-changing requests.
  - **`SameSite` Cookies:** Set the `SameSite` attribute on cookies to `Lax` or `Strict`.

- **Clickjacking:**
  - **`frame-ancestors`:** Use the `Content-Security-Policy: frame-ancestors 'none';` directive to prevent the site from being framed.

- **DOM-based XSS:**
  - **Avoid Dangerous Sinks:** Do not use `innerHTML`, `document.write`, or `eval` with untrusted data.
  - **Trusted Types:** Enforce Trusted Types via CSP to prevent the use of string-based DOM manipulation APIs.

## File and Data Handling

- **File Uploads:**
  - **Validate Everything:** Validate file extensions, content types (using magic numbers, not just the `Content-Type` header), and file content.
  - **Secure Storage:** Store uploaded files outside of the web root, on a separate server if possible.
  - **Generate Filenames:** Use randomly generated filenames (e.g., UUIDs) instead of user-supplied filenames.
  - **Virus Scanning:** Scan all uploaded files for malware.

- **XML and Deserialization:**
  - **Disable External Entities (XXE):** Disable DTDs and external entities in XML parsers to prevent XML External Entity (XXE) attacks.
  - **Safe Deserialization:** Avoid deserializing untrusted data from native formats (e.g., Java serialization, Python's `pickle`). Prefer data formats like JSON and use schema validation.
  - **Resource Limiting:** Set limits on the size and complexity of parsed documents to prevent denial-of-service attacks.
