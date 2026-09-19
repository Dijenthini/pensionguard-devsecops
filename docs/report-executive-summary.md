# Executive Summary - Report
# Date: 2026-09-19
# Member 1 - System/Container Lead

## Executive Summary

The PensionGuard project demonstrates the practical application of DevSecOps principles to secure a real-world web application. Using OWASP NodeGoat, an intentionally vulnerable retirement savings management platform, the team identified, exploited, fixed, and verified four critical security vulnerabilities while building an automated CI/CD pipeline with five security gates.

### Project Scope

The application, built with Node.js, Express, and MongoDB, was containerized using Docker and deployed through a GitHub Actions pipeline. The team applied the STRIDE threat modeling methodology to identify eight potential threats, prioritised by risk, and implemented controls for the highest-priority issues.

### Vulnerabilities Addressed

Four vulnerabilities were successfully remediated:

| Vulnerability | Risk Level | Status |
|---------------|------------|--------|
| Server-Side JavaScript Injection (eval) | Critical | ✅ Fixed |
| Stored XSS | Medium | ✅ Fixed |
| IDOR (Allocations) | High | ✅ Fixed |
| Broken Access Control (Benefits) | Medium | ✅ Fixed |

Each vulnerability was demonstrated working before the fix, then verified blocked after remediation, with full evidence captured.

### Security Pipeline

The CI/CD pipeline includes five automated security gates:

| Gate | Tool | Purpose |
|------|------|---------|
| SAST | Semgrep | Detect code vulnerabilities (blocking) |
| Dependency Scan | npm audit | Find vulnerable dependencies |
| Secrets Scan | Gitleaks | Detect exposed secrets |
| Container Scan | Trivy | Scan Docker images |
| Build & Test | npm | Verify code functionality |

The SAST gate is configured to **block the pipeline** when vulnerabilities are detected, as proven through RED and GREEN run evidence.

### Results

| Metric | Value |
|--------|-------|
| Vulnerabilities Fixed | 4 |
| Threats Identified | 8 |
| Security Gates | 5 |
| SAST Findings (Before → After) | 6 → 0 |
| Secrets Exposed | 0 |
| RED Runs Captured | ✅ Yes |
| GREEN Runs Captured | ✅ Yes |

### Conclusion

PensionGuard demonstrates that a small application can implement enterprise-grade DevSecOps practices using free, open-source tools. The project provides a repeatable template for secure software delivery, combining threat modeling, secure coding, automated security scanning, and secrets management.

The complete solution is reproducible with a single command (`docker compose up --build -d`) and all evidence is documented in the project repository.