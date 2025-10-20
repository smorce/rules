# Encryption

This document summarizes best practices for encryption, including algorithm selection, key management, data protection, and certificate validation.

## Algorithms and Modes

- **Symmetric Encryption:** Use modern authenticated encryption with associated data (AEAD) ciphers.
  - **Recommended:** AES-GCM, ChaCha20-Poly1305.
  - **Avoid:** Older modes like ECB, which is insecure. CBC and CTR are acceptable only if combined with a strong MAC (Encrypt-then-MAC).

- **Asymmetric Encryption:** Employ strong, widely-supported algorithms.
  - **Recommended:** RSA with keys of 2048 bits or more, or modern Elliptic Curve Cryptography (ECC) such as Curve25519 or Ed25519.
  - **Padding:** Use OAEP for RSA encryption.

- **Hashing:** Select secure hash algorithms for integrity checks.
  - **Recommended:** SHA-256 or stronger variants from the SHA-2 or SHA-3 families.
  - **Forbidden:** MD5 and SHA-1, as they are cryptographically broken.

- **Random Number Generation:** Always use a Cryptographically Secure Pseudo-Random Number Generator (CSPRNG) for generating keys, nonces, and IVs. Examples include `SecureRandom` in Java, `crypto.randomBytes` in Node.js, and the `secrets` module in Python.

## Key Management

- **Generation:** Generate cryptographic keys within validated hardware security modules (HSMs) or managed key services (KMS). Never derive keys from passwords or predictable inputs.
- **Segregation:** Use separate keys for distinct purposes (e.g., encryption vs. signing).
- **Storage:** Store keys securely in a KMS, HSM, or a dedicated vault. Never hardcode keys in source code or store them in plaintext configuration files. Use Key Encryption Keys (KEKs) to wrap Data Encryption Keys (DEKs).
- **Lifecycle:** Rotate keys upon compromise, at the end of their defined cryptoperiod, or according to organizational policy.

## Data Protection

- **Data at Rest:** Encrypt sensitive data stored in databases, files, and backups. Use authenticated encryption and manage initialization vectors (IVs) or nonces correctly.
- **Data in Transit:** Enforce HTTPS across all services to protect data transmitted over networks.
  - **TLS:** Use TLS 1.3 or 1.2. Disable outdated protocols like SSLv3, TLS 1.0, and TLS 1.1.
  - **HSTS:** Implement HTTP Strict Transport Security (HSTS) to prevent downgrade attacks.
  - **Certificate Pinning:** Use pinning with caution, primarily for controlled clients like mobile apps, and ensure a secure update channel for pins.

## Certificate Validation

When handling X.509 certificates, perform the following checks:

1.  **Expiration:**
    - Reject certificates that are expired or not yet valid.
2.  **Key Strength:**
    - Ensure public keys meet minimum strength requirements: RSA keys must be at least 2048 bits, and EC keys must use a curve of at least 256 bits (e.g., P-256).
3.  **Signature Algorithm:**
    - Verify that the certificate is signed with a secure algorithm (e.g., `sha256WithRSAEncryption`). Reject signatures based on MD5 or SHA-1.
4.  **Issuer:**
    - Identify self-signed certificates (where the `Issuer` and `Subject` are identical) and ensure they are only used in appropriate contexts (e.g., development or internal services).
