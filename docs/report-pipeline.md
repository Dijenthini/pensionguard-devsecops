# CI/CD Pipeline Section - Draft
# Date: 2026-09-13
# Member 4 - Pipeline/Evidence Lead

## 1. Introduction

The PensionGuard project uses **GitHub Actions** to implement a complete DevSecOps pipeline with **five security gates**. The pipeline runs automatically on every push and pull request, ensuring that no insecure code reaches the main branch.

---

## 2. Pipeline Architecture

The pipeline consists of five jobs:

| Job | Tool | Blocking? | Purpose |
|-----|------|-----------|---------|
| Build & Test | npm | No | Verify code builds successfully |
| SAST | Semgrep | ✅ **BLOCKING** | Detect code vulnerabilities |
| Dependency Scan | npm audit | No | Find vulnerable dependencies |
| Secrets Scan | Gitleaks | No | Detect exposed secrets |
| Container Scan | Trivy | No | Scan Docker image |

---

## 3. Security Gates

### 3.1 SAST (Semgrep) - BLOCKING GATE

- **Tool:** Semgrep
- **Config:** `r/javascript.lang.security.audit.eval-detected`
- **Blocking:** ✅ Yes (`continue-on-error: false`)
- **Evidence:** EVID-24 (RED run) and EVID-25 (GREEN run)

This gate **blocks the pipeline** if `eval()` is detected in the code, preventing code injection vulnerabilities.

### 3.2 Dependency Scanning

- **Tool:** npm audit
- **Severity:** HIGH
- **Report:** Saves findings to artifact

### 3.3 Secrets Scanning

- **Tool:** Gitleaks
- **Scope:** Full git history
- **Detection:** API keys, passwords, tokens

### 3.4 Container Scanning

- **Tool:** Trivy
- **Severity:** CRITICAL, HIGH
- **Image:** `pensionguard:test`

---

## 4. Blocking Gate Demonstration

### 4.1 RED Run

The pipeline was deliberately made to fail by introducing an `eval()` in `contributions.js`. The SAST gate detected this and blocked the pipeline.

**Evidence:** EVID-24-pipeline-red-run.png (44 findings, exit code 1)

### 4.2 GREEN Run

After removing the test vulnerability, the pipeline passed successfully.

**Evidence:** EVID-25-pipeline-green-run.png

---

## 5. Secrets Management

All secrets are managed through **GitHub Actions encrypted secrets**. No credentials are hardcoded or committed to the repository.

| Secret | Purpose |
|--------|---------|
| `SESSION_SECRET` | Session encryption |
| `DB_PASSWORD` | Database password |
| `SEMGREP_APP_TOKEN` | Semgrep API token |

Secrets are injected at runtime via `${{ secrets.NAME }}` and are never visible in logs or code.

---

## 6. Summary

| Metric | Value |
|--------|-------|
| Total Jobs | 5 |
| Blocking Gates | 1 (SAST) |
| Scans Run per Push | 5 |
| Secrets Exposed | 0 |
| RED Run Captured | ✅ Yes |
| GREEN Run Captured | ✅ Yes |

The pipeline demonstrates a complete DevSecOps workflow with automated security controls integrated at every stage.