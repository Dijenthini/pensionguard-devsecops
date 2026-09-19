# Pipeline Summary - PensionGuard
# Date: 2026-09-19
# Member 4 - Pipeline/Evidence Lead

## 1. Pipeline Overview

| Metric | Value |
|--------|-------|
| Total Jobs | 5 |
| Blocking Gates | 1 (SAST) |
| Security Tools | 5 |
| Secrets Exposed | 0 |
| RED Run Captured | ✅ Yes |
| GREEN Run Captured | ✅ Yes |

## 2. Pipeline Jobs

| Job | Tool | Blocking? | Purpose |
|-----|------|-----------|---------|
| Build & Test | npm | No | Verify code builds |
| SAST | Semgrep | ✅ **YES** | Detect code vulnerabilities |
| Dependency Scan | npm audit | No | Find vulnerable dependencies |
| Secrets Scan | Gitleaks | No | Detect exposed secrets |
| Container Scan | Trivy | No | Scan Docker image |

## 3. Evidence Summary

| Evidence | File | What It Shows |
|----------|------|---------------|
| EVID-23 | `EVID-23-pipeline-basic.png` | Basic pipeline (3 jobs) |
| EVID-24 | `EVID-24-pipeline-red-run.png` | Pipeline FAILED (SAST blocking) |
| EVID-24b | `EVID-24b-sast-failure-details.png` | SAST failure details |
| EVID-25 | `EVID-25-pipeline-green-run.png` | Pipeline PASSED |
| EVID-26 | `EVID-26-pipeline-final-green.png` | Final verified green run |

## 4. Blocking Gate Details

- **Gate:** SAST (Semgrep)
- **Rule:** Local rule `.semgrep/eval-detected.yml`
- **Behavior:** Non-zero exit code blocks pipeline
- **Proof:** EVID-24 (RED run), EVID-25 (GREEN run)

## 5. Secrets Management

All secrets managed through GitHub Actions encrypted secrets:

| Secret | Purpose |
|--------|---------|
| `SESSION_SECRET` | Session encryption |
| `DB_PASSWORD` | Database password |
| `SEMGREP_APP_TOKEN` | Semgrep API token |

## 6. Pipeline Configuration

**File:** `.github/workflows/devsecops.yml`

**Triggers:**
- `push` to master, main, demo/*
- `pull_request` to master, main
- `workflow_dispatch` (manual)

**Actions Used:**
- `actions/checkout@v4`
- `actions/setup-node@v4`
- `aquasecurity/trivy-action@master`
- `gitleaks/gitleaks-action@v2`

## 7. Summary

The pipeline demonstrates a complete DevSecOps workflow with automated security controls integrated at every stage. The SAST blocking gate proved it can fail the pipeline when a vulnerability is introduced, and the pipeline passes when the code is secure.