# CI/CD Pipeline Section - Report
# Date: 2026-09-14
# Member 4 - Pipeline/Evidence Lead

## 1. Introduction

The PensionGuard project uses **GitHub Actions** to implement a comprehensive DevSecOps pipeline with **five automated security gates**. The pipeline runs on every push and pull request, ensuring that no insecure code reaches the main branch.

## 2. Pipeline Architecture

The pipeline consists of five jobs running in parallel:

| Job | Tool | Blocking? | Purpose |
|-----|------|-----------|---------|
| Build & Test | npm | No | Verify code builds and tests pass |
| SAST | Semgrep | ✅ **BLOCKING** | Detect code vulnerabilities |
| Dependency Scan | npm audit | No | Find vulnerable dependencies |
| Secrets Scan | Gitleaks | No | Detect exposed secrets |
| Container Scan | Trivy | No | Scan Docker image |

## 3. Security Gates Detail

### 3.1 SAST - Semgrep (BLOCKING GATE)

- **Tool:** Semgrep Community Edition
- **Rule:** `r/javascript.lang.security.audit.eval-detected`
- **Blocking:** ✅ Yes (`continue-on-error: false`)
- **Exit behavior:** Non-zero exit code blocks pipeline

**Why blocking:** This gate prevents code injection vulnerabilities from entering the codebase.

**Evidence:**
- RED run: EVID-24-pipeline-red-run.png (44 findings, exit code 1)
- GREEN run: EVID-25-pipeline-green-run.png (0 findings, pass)

### 3.2 Dependency Scanning

- **Tool:** npm audit
- **Severity threshold:** HIGH
- **Report:** JSON output saved to artifacts

### 3.3 Secrets Scanning

- **Tool:** Gitleaks
- **Scope:** Full git history (`fetch-depth: 0`)
- **Detection:** API keys, passwords, tokens, private keys

### 3.4 Container Scanning

- **Tool:** Trivy
- **Image:** `pensionguard:test`
- **Severity:** CRITICAL, HIGH

## 4. Blocking Gate Demonstration

### 4.1 RED Run

To demonstrate the blocking gate, an `eval()` was temporarily introduced in `app/routes/contributions.js`.

**Result:** 
- SAST detected eval
- Pipeline FAILED
- 44 findings (44 blocking)
- Exit code 1

**Evidence:** EVID-24-pipeline-red-run.png, EVID-24b-sast-failure-details.png

### 4.2 GREEN Run

After removing the test vulnerability, the pipeline passed.

**Result:**
- All 5 jobs passed
- Pipeline completed in 1m 17s

**Evidence:** EVID-25-pipeline-green-run.png

## 5. Secrets Management

All secrets are managed through **GitHub Actions encrypted secrets**:

| Secret | Purpose |
|--------|---------|
| `SESSION_SECRET` | Session encryption |
| `DB_PASSWORD` | Database password |
| `SEMGREP_APP_TOKEN` | Semgrep API token |

**Provisioning:**
- CI/CD: `${{ secrets.NAME }}` injection
- Application: `process.env.NAME`
- No hardcoded credentials anywhere

## 6. Workflow Configuration

**File:** `.github/workflows/devsecops.yml`

**Triggers:**
- `push` to master, main, demo/*
- `pull_request` to master, main
- `workflow_dispatch` (manual)

**Actions used:**
- `actions/checkout@v4`
- `actions/setup-node@v4`
- `aquasecurity/trivy-action@master`
- `gitleaks/gitleaks-action@v2`

## 7. Summary

| Metric | Value |
|--------|-------|
| Total Jobs | 5 |
| Blocking Gates | 1 (SAST) |
| Security Tools | 5 (npm, Semgrep, npm audit, Gitleaks, Trivy) |
| Secrets Exposed | 0 |
| RED Run Captured | ✅ Yes |
| GREEN Run Captured | ✅ Yes |

The pipeline demonstrates a complete DevSecOps workflow with automated security controls integrated at every stage of the software delivery lifecycle.