# Industry Trends and Case Study - Report
# Date: 2026-09-19
# Member 2 - Threat Modeling Lead

## 1. Introduction

This section examines current DevSecOps industry trends and analyzes a recent supply chain attack case study. The lessons learned are connected to the PensionGuard project's security controls and demonstrate the practical value of the implemented pipeline.

## 2. Current DevSecOps Trends

### 2.1 Shift-Left Security

Security is increasingly integrated into early development stages rather than being a final testing phase. In PensionGuard, this is demonstrated by:
- Threat modeling during design (STRIDE analysis)
- Secure coding during implementation
- SAST scanning on every commit
- Security gates blocking insecure builds

### 2.2 Security Automation

Manual security reviews do not scale with modern development velocity. Automation is essential:
- Automated SAST, SCA, secrets, and container scanning in the pipeline
- Automated blocking of insecure builds
- No manual intervention required for security checks
- Consistent security controls across every build

### 2.3 Software Supply Chain Security

Dependencies and build pipelines are prime attack targets. Our controls include:
- Dependency scanning with npm audit
- Container image scanning with Trivy
- Secrets scanning with Gitleaks
- Pinned GitHub Actions versions (avoiding mutable tags)

### 2.4 Infrastructure as Code (IaC) Security

Infrastructure defined in code must be secured. Our project uses:
- Docker Compose for orchestration
- Container hardening (non-root user, resource limits)
- Network isolation (Docker internal network)
- Minimal base images (node:12-alpine)

## 3. Case Study: SolarWinds Supply Chain Attack (2020)

### 3.1 Attack Overview

In December 2020, attackers compromised SolarWinds' build pipeline and injected malicious code into Orion software updates. The compromised updates were distributed to over 18,000 customers, including US government agencies. This attack is considered one of the most significant supply chain breaches in history.

### 3.2 Attack Vector

| Stage | What Happened |
|-------|---------------|
| *Initial Access* | Attackers compromised the build environment |
| *Persistence* | Malicious code added to legitimate software updates |
| *Distribution* | Updates signed with legitimate certificates |
| *Impact* | Backdoor installed on 18,000+ systems |
| *Detection* | Took months to discover |

### 3.3 Root Causes

| Cause | Description |
|-------|-------------|
| *Compromised pipeline* | Build system was not adequately secured |
| *No integrity verification* | Updates not verified for tampering |
| *Limited visibility* | No SBOM or dependency tracking |
| *Weak access controls* | Build credentials not properly protected |
| *No automated security gates* | Insecure code not detected before distribution |

### 3.4 Key Lessons

| Lesson | Description |
|--------|-------------|
| *Pipeline security matters* | Build systems are critical attack surfaces |
| *Verify artifacts* | Container images should be scanned |
| *Track dependencies* | Vulnerable packages must be identified |
| *Protect secrets* | Credentials must never leak |
| *Block insecure builds* | Security gates prevent bad code from shipping |

## 4. Connection to PensionGuard

### 4.1 Supply Chain Security Comparison

| Threat | SolarWinds | PensionGuard |
|--------|------------|--------------|
| Compromised dependencies | ❌ Not detected | ✅ npm audit |
| Malicious updates | ❌ Not verified | ✅ Trivy scan |
| Build pipeline attack | ❌ Compromised | ✅ Pinned Actions |
| Secret leakage | ❌ Undetected | ✅ Gitleaks |
| Insecure code shipped | ❌ Not blocked | ✅ SAST blocking gate |

### 4.2 Security Controls Implemented

| Control | Purpose | SolarWinds Gap |
|---------|---------|----------------|
| SAST (Semgrep) | Detect code vulnerabilities | ❌ Missing |
| Dependency Scan (npm audit) | Find vulnerable dependencies | ❌ Missing |
| Secrets Scan (Gitleaks) | Detect exposed secrets | ❌ Missing |
| Container Scan (Trivy) | Verify image integrity | ❌ Missing |
| Blocking Gate | Prevent insecure builds | ❌ Missing |
| Pinned Actions | Prevent supply chain attacks | ❌ Missing |

### 4.3 Key Takeaways

The SolarWinds attack demonstrates that:
1. *Build pipelines are critical attack surfaces*
2. *Dependency and container scanning are essential*
3. *Automated security gates prevent insecure code from shipping*
4. *Visibility into the supply chain is necessary*
5. *Pinning dependencies prevents tampering*

PensionGuard's pipeline implements these lessons through automated security controls at every stage of the delivery process.

## 5. Industry Trend Analysis

### 5.1 Shift-Left Security Adoption

| Trend | PensionGuard Implementation |
|-------|----------------------------|
| Threat modeling early | STRIDE analysis during design |
| Secure coding | Input validation, output encoding |
| Automated security testing | 5 security gates in pipeline |
| Developer security awareness | Cross-trained team |

### 5.2 Supply Chain Security Growth

| Trend | PensionGuard Implementation |
|-------|----------------------------|
| SBOM (Software Bill of Materials) | Container scanning with Trivy |
| Dependency scanning | npm audit on every push |
| Pinned dependencies | Locked versions in package-lock.json |
| Artifact verification | Image scanning in pipeline |

### 5.3 DevSecOps Automation

| Trend | PensionGuard Implementation |
|-------|----------------------------|
| CI/CD security integration | GitHub Actions |
| Automated blocking | SAST gate blocks pipeline |
| Continuous scanning | Every push and pull request |
| Secrets management | GitHub Actions encrypted secrets |

## 6. Conclusion

The SolarWinds attack was a watershed moment for DevSecOps, demonstrating that supply chain security is critical. PensionGuard applies these lessons through automated security controls including SAST, dependency scanning, secrets scanning, and container scanning — with a blocking gate to prevent insecure code from being deployed.

The project demonstrates how industry best practices can be applied to a small application, providing a template for secure software delivery that addresses modern supply chain threats.

## 7. References

- [1] SolarWinds Security Advisory, "SolarWinds Orion Supply Chain Attack," 2020.
- [2] OWASP Top 10:2025. Available: https://owasp.org/Top10/
- [3] NIST SP 800-204D, "Strategies for the Integration of Software Supply Chain Security," 2022.
- [4] GitHub Actions Security Best Practices. Available: https://docs.github.com/actions/security-guides
- [5] Semgrep Documentation. Available: https://semgrep.dev/docs/
- [6] Trivy Documentation. Available: https://github.com/aquasecurity/trivy
- [7] Gitleaks. Available: https://github.com/gitleaks/gitleaks