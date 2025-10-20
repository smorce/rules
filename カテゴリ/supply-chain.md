# Supply Chain Security

This document provides a summary of best practices for securing the software supply chain, from managing third-party dependencies to ensuring the integrity of build and deployment processes.

## Dependency Management

- **Use Lockfiles:** Always use a lockfile (e.g., `package-lock.json`, `yarn.lock`, `Pipfile.lock`) to ensure that builds are deterministic and use the exact same versions of dependencies.
- **Version Pinning:** Pin dependency versions to specific, trusted releases. Avoid using version ranges that could automatically introduce a malicious or vulnerable update.
- **Private Registries:** Use a private, trusted registry for internal packages and consider proxying or mirroring public registries to have more control over the dependencies that are used.
- **Minimize Dependencies:** Regularly review and remove unused dependencies to reduce the attack surface.

## Vulnerability Management

- **Scanning:** Integrate Software Composition Analysis (SCA) tools into the CI/CD pipeline to automatically scan for known vulnerabilities in dependencies.
- **Patching:** Establish a process for promptly updating dependencies when security patches are released.
- **Compensating Controls:** For vulnerabilities that cannot be patched immediately, implement compensating controls, such as input validation or runtime protection, to mitigate the risk.

## Build and Deployment Integrity

- **Software Bill of Materials (SBOM):** Generate an SBOM for every build. An SBOM is a formal, machine-readable inventory of the software components and dependencies used in an application.
- **Provenance:** Establish the provenance of build artifacts, meaning a record of where they came from and how they were produced. Use frameworks like SLSA (Supply-chain Levels for Software Artifacts) to attest to the integrity of the build process.
- **Artifact Signing:** Digitally sign build artifacts to ensure their authenticity and integrity. Verify these signatures before deployment.
- **Hermetic Builds:** Whenever possible, perform builds in an isolated environment with no network access to prevent tampering.

## Developer Practices

- **Typosquatting Awareness:** Be cautious of typosquatting, where attackers publish malicious packages with names that are similar to popular packages.
- **Review Install Scripts:** Be aware of and review any scripts that are executed automatically when a package is installed.
- **Secure Publishing:** When publishing packages, use two-factor authentication (2FA) for the package registry account.
