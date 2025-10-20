# Cloud Security

This document provides a summary of best practices for securing cloud environments, including Infrastructure as Code (IaC), container orchestration with Kubernetes, and CI/CD pipelines.

## Infrastructure as Code (IaC) Security

- **Network Security:**
  - **Restrict Access:** Never expose administrative ports (e.g., SSH, RDP) or database ports to the entire internet (`0.0.0.0/0`). Use allow-lists of specific IP addresses or CIDR blocks.
  - **Private Networking:** Prefer private networking (e.g., VPCs, VNETs) and only expose services to the public internet when absolutely necessary.
  - **Default Deny:** Implement a "default deny" firewall policy and only allow traffic that is explicitly required.

- **Data Protection:**
  - **Encryption at Rest:** Enable encryption at rest for all storage services, including object storage (e.g., S3, GCS), block storage (e.g., EBS), and databases (e.g., RDS).
  - **Encryption in Transit:** Enforce the use of TLS 1.2 or higher for all network communication.

- **Access Control:**
  - **Least Privilege:** Do not use wildcard permissions (e.g., `"Action": "*", "Resource": "*"`) in IAM policies. Grant only the minimum necessary permissions.
  - **Workload Identity:** Whenever possible, use workload identity (e.g., IAM Roles for Service Accounts) instead of long-lived credentials like API keys.
  - **No Hardcoded Secrets:** Never hardcode secrets in IaC files. Use a secret management service like AWS Secrets Manager or HashiCorp Vault.

## Kubernetes Security

- **Identity and Access Control (RBAC):**
  - **Least Privilege:** Apply the principle of least privilege to both user accounts and service accounts. Use separate namespaces to isolate applications.
  - **Audit Logging:** Enable audit logging to record all API requests to the Kubernetes API server.

- **Network Security:**
  - **Network Policies:** Use network policies to control traffic between pods. Implement a "default deny" policy and only allow necessary communication.
  - **Service Mesh:** For more advanced traffic control and security, consider using a service mesh like Istio or Linkerd to provide mutual TLS (mTLS) between services.

- **Admission Control:**
  - **Policy Enforcement:** Use admission controllers (e.g., OPA/Gatekeeper, Kyverno) to enforce security policies, such as requiring that all images come from a trusted registry or that containers do not run as root.

- **Secrets Management:**
  - **Use KMS:** Integrate with a Key Management Service (KMS) to encrypt Kubernetes secrets at rest.
  - **Rotate Secrets:** Regularly rotate secrets.

## Container and CI/CD Security

- **Container Hardening:**
  - **Minimal Base Images:** Use minimal base images (e.g., distroless, Alpine) to reduce the attack surface.
  - **Non-Root User:** Run containerized applications as a non-root user.
  - **Read-Only Root Filesystem:** Whenever possible, run containers with a read-only root filesystem.

- **CI/CD Pipeline Security:**
  - **Scan Everything:** Integrate security scanning tools into the CI/CD pipeline, including:
    - **Static Application Security Testing (SAST):** Scans source code for vulnerabilities.
    - **Software Composition Analysis (SCA):** Scans for known vulnerabilities in third-party dependencies.
    - **Dynamic Application Security Testing (DAST):** Scans a running application for vulnerabilities.
    - **IaC Scanning:** Scans IaC files for misconfigurations.
  - **Sign Artifacts:** Digitally sign all build artifacts (e.g., container images, JAR files) and verify the signatures before deployment.
