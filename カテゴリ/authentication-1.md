# Authentication Part 1: Credentials and Sessions

This document covers best practices for user authentication, including password security, multi-factor authentication (MFA), and secure session management.

## User Credentials

- **Password Policies:**
  - **Strength:** Encourage passphrases with a minimum length of 8 characters and support for Unicode. Avoid complex composition rules.
  - **Breached Password Detection:** Check new passwords against known data breaches and reject common or compromised passwords.

- **Password Storage:**
  - **Hashing:** Always hash passwords using a slow, memory-hard algorithm with a unique salt for each user.
  - **Recommended Algorithms:**
    1.  **Argon2id** (preferred)
    2.  **scrypt**
    3.  **bcrypt** (for legacy systems)
    4.  **PBKDF2-HMAC-SHA-256** (when FIPS compliance is required)
  - **Constant-Time Comparison:** Use a constant-time comparison function to verify password hashes to prevent timing attacks.

- **Authentication Flow:**
  - **Generic Error Messages:** Return generic error messages for failed login attempts to prevent account enumeration.
  - **Rate Limiting:** Implement rate limiting on authentication endpoints to defend against brute-force attacks.

## Multi-Factor Authentication (MFA)

- **Phishing-Resistant Factors:** Prioritize phishing-resistant MFA methods.
  - **Strongest:** Passkeys (WebAuthn/FIDO2), U2F hardware tokens.
  - **Acceptable:** Time-based One-Time Passwords (TOTP) from authenticator apps.
  - **Avoid:** SMS, voice calls, or email for delivering MFA codes, as they are vulnerable to interception.

- **MFA Triggers:** Require MFA for sensitive actions, including:
  - Logging in
  - Changing a password or email address
  - Disabling MFA
  - High-value transactions

- **Recovery:** Provide secure MFA recovery options, such as single-use backup codes.

## Session Management

- **Session ID Generation:**
  - **Entropy:** Generate session IDs using a Cryptographically Secure Pseudo-Random Number Generator (CSPRNG) with at least 128 bits of entropy.
  - **Opaqueness:** Session IDs should be opaque and not contain any meaningful information.

- **Secure Cookies:**
  - **Attributes:** Set the `Secure`, `HttpOnly`, and `SameSite=Strict` attributes on session cookies.
  - **Scope:** Scope cookies to a specific path and domain.
  - **HTTPS:** Enforce HTTPS for all communication to protect session cookies in transit.

- **Session Lifecycle:**
  - **Rotation:** Regenerate the session ID upon any change in privilege level, such as logging in or changing a password. Invalidate the old session ID.
  - **Timeouts:** Enforce both idle and absolute session timeouts on the server side.
  - **Logout:** Provide a clearly visible logout button that invalidates the session on the server.

- **Session Hijacking Prevention:**
  - **Fingerprinting:** On the server side, track context attributes like IP address and User-Agent.
  - **Anomaly Detection:** If a request's fingerprint does not match the session's stored fingerprint, require re-authentication or invalidate the session.
