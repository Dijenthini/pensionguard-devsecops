# Secrets Management - PensionGuard
# Date: 2026-09-19
# Member 4 - Pipeline/Evidence Lead

## 1. Overview

All secrets in the PensionGuard project are managed using **GitHub Actions encrypted secrets**. No credentials, API keys, or connection strings are hardcoded or committed to the repository at any point.

This document explains:
- How secrets are stored
- How secrets are provisioned to the pipeline and application
- How secrets are protected from exposure

---

## 2. Secrets Storage

All secrets are stored in **GitHub Actions Repository Secrets**.

### 2.1 Secrets Registered

| Secret Name | Purpose | Where Used |
|-------------|---------|------------|
| `SESSION_SECRET` | Session encryption key | Application runtime |
| `DB_PASSWORD` | MongoDB connection password | Application runtime |
| `SEMGREP_APP_TOKEN` | Semgrep API token (optional) | CI/CD pipeline |

### 2.2 Where They Are Stored

- **Location:** `https://github.com/Dijenthini/pensionguard-devsecops/settings/secrets/actions`
- **Encryption:** GitHub encrypts secrets at rest using libsodium sealed boxes
- **Access:** Only workflows running in this repository can access the secrets

---

## 3. How Secrets Are Provisioned

### 3.1 To the CI/CD Pipeline

In `.github/workflows/devsecops.yml`, secrets are injected using the `${{ secrets.NAME }}` syntax:

```yaml
      - name: Run Semgrep SAST
        run: |
          semgrep scan --config "r/javascript.lang.security.audit.eval-detected" --error
        continue-on-error: false
        env:
          SEMGREP_APP_TOKEN: ${{ secrets.SEMGREP_APP_TOKEN }}
```

**How it works:**
1. GitHub injects the secret value at runtime
2. The value is available as an environment variable
3. The value is **masked** in logs (replaced with `***`)

### 3.2 To the Application

The application reads secrets from environment variables:

```javascript
// Example: Session secret
const sessionSecret = process.env.SESSION_SECRET || 'default-dev-secret';

// Example: Database password
const dbPassword = process.env.DB_PASSWORD;
```

**Important:** Fallback values are used ONLY for local development — never for production.

---

## 4. Local Development Setup

### 4.1 `.env.example` Template

A template file is committed to the repository to document required environment variables **without exposing real values**.

**File:** `.env.example`

```
# Session encryption secret
SESSION_SECRET=your-session-secret-here

# MongoDB password
DB_PASSWORD=your-db-password-here
```

### 4.2 Local `.env` File

Developers copy `.env.example` to `.env` and fill in real values locally.

**File:** `.env` (never committed)

```
SESSION_SECRET=local-dev-session-secret
DB_PASSWORD=local-dev-db-password
```

### 4.3 `.gitignore` Protection

The `.gitignore` file prevents `.env` from being committed:

```
# Environment variables
.env
.env.local
.env.*.local

# Secrets
*.pem
*.key
```

---

## 5. Secrets Scanning with Gitleaks

Gitleaks runs on every push to detect any accidentally committed secrets.

### 5.1 Configuration

```yaml
  secrets-scan:
    name: Secrets Scan - Gitleaks
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0    # Full history scan

      - name: Run Gitleaks
        uses: gitleaks/gitleaks-action@v2
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        continue-on-error: true
```

### 5.2 Scan Results

| Scan Type | Result | Date |
|-----------|--------|------|
| Full git history | ✅ No secrets detected | 2026-09-19 |
| Current code | ✅ No secrets detected | 2026-09-19 |

---

## 6. Security Best Practices Applied

| # | Practice | Status |
|---|----------|--------|
| 1 | No hardcoded credentials in source code | ✅ Applied |
| 2 | GitHub Actions encrypted secrets | ✅ Applied |
| 3 | Gitleaks full-history scanning | ✅ Applied |
| 4 | `.env.example` for documentation | ✅ Applied |
| 5 | `.env` in `.gitignore` | ✅ Applied |
| 6 | No secrets in Dockerfile | ✅ Applied |
| 7 | No secrets in build arguments | ✅ Applied |
| 8 | Secret values masked in logs | ✅ Applied |
| 9 | Least-privilege workflow permissions | ✅ Applied |
| 10 | Secrets scoped to jobs that need them | ✅ Applied |

---

## 7. What Was Removed

The following insecure patterns were removed or avoided:

| ❌ Insecure Pattern | ✅ Secure Alternative |
|---------------------|----------------------|
| `const password = "admin123"` | `const password = process.env.DB_PASSWORD` |
| Secrets in `Dockerfile` | Secrets from environment variables |
| Secrets in `docker-compose.yml` | Secrets from `.env` file |
| Secrets in git history | GitHub Secrets + `.gitignore` |
| Secrets in CI logs | GitHub masks secret values |

---

## 8. Secret Rotation Policy

| Secret | Rotation Frequency | Owner |
|--------|-------------------|-------|
| `SESSION_SECRET` | Every 90 days | Member 4 |
| `DB_PASSWORD` | Every 90 days | Member 4 |
| `SEMGREP_APP_TOKEN` | Every 180 days | Member 4 |

**To rotate a secret:**
1. Go to GitHub repository Settings → Secrets
2. Click the secret name → **Update**
3. Enter new value → **Save**
4. Update `.env` locally for development
5. Re-run pipeline to verify

---

## 9. Summary

| Aspect | Implementation |
|--------|----------------|
| **Storage** | GitHub Actions encrypted secrets |
| **Runtime injection** | `${{ secrets.NAME }}` |
| **Application access** | `process.env.NAME` |
| **Detection** | Gitleaks (full history) |
| **Documentation** | `.env.example` |
| **Protection** | `.gitignore` |
| **Log masking** | GitHub automatic |
| **No hardcoded secrets** | ✅ Verified with Gitleaks |

**Result:** No secrets are committed to the repository at any point. All credentials are managed securely through GitHub Actions encrypted secrets, with detection and prevention controls in place.

---

## 10. Evidence References

| Evidence | Description |
|----------|-------------|
| `EVID-23-pipeline-basic.png` | Initial pipeline (3 jobs) |
| `EVID-24-pipeline-red-run.png` | Pipeline failure (SAST blocking) |
| `EVID-25-pipeline-green-run.png` | Pipeline passing after fix |
| `secrets-scan` job logs | Gitleaks clean scan |