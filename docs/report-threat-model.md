# Threat Model Section - Report
# Date: 2026-09-19
# Member 2 - Threat Modeling Lead

## 1. Introduction

The PensionGuard application (OWASP NodeGoat) was analyzed using the *STRIDE threat modeling methodology*. This section presents the assets, actors, trust boundaries, identified threats, risk assessment, and applied controls.

## 2. Assets and Actors

### 2.1 Assets

| Asset | Description | Sensitivity |
|-------|-------------|-------------|
| User credentials | Usernames, passwords | High |
| Session cookies | Authentication tokens | High |
| Personal profiles | Name, email, SSN | High |
| Financial data | Retirement allocations, contributions | High |
| Admin functions | Benefits management | High |
| Source code | Application logic | Medium |

### 2.2 Actors

| Actor | Description |
|-------|-------------|
| Unauthenticated visitor | External user with no account |
| Regular user | Authenticated non-admin |
| Administrator | Authenticated admin |
| Malicious insider | Privileged user acting badly |

## 3. Trust Boundaries

| Boundary | Separates | Risk |
|----------|-----------|------|
| B1: Internet → Internal | User browser vs web app | Malicious input |
| B2: App → Database | Web app vs MongoDB | Query injection |
| B3: User → Admin | Regular vs admin | Privilege escalation |

## 4. STRIDE Threat Analysis

Eight threats were identified across six STRIDE categories:

| ID | Threat | STRIDE | Likelihood | Impact | Score | Level | Status |
|----|--------|--------|------------|--------|-------|-------|--------|
| T1 | Weak Authentication | S | 3 | 4 | 12 | HIGH | ⏳ Pending |
| T2 | Benefits Access | E | 3 | 3 | 9 | MEDIUM | ✅ Fixed |
| T3 | Stored XSS | T | 3 | 3 | 9 | MEDIUM | ✅ Fixed |
| T4 | No Audit Logs | R | 3 | 2 | 6 | MEDIUM | ⏳ Pending |
| T5 | IDOR (Allocations) | I | 4 | 4 | 16 | HIGH | ✅ Fixed |
| T6 | Error Disclosure | I | 2 | 2 | 4 | MEDIUM | ⏳ Pending |
| T7 | Denial of Service | D | 2 | 3 | 6 | MEDIUM | ⏳ Pending |
| T8 | eval Injection | T/E | 4 | 5 | 20 | CRITICAL | ✅ Fixed |

## 5. Risk Assessment

### 5.1 5x5 Likelihood/Impact Matrix

| Impact / Likelihood | 1 | 2 | 3 | 4 | 5 |
|---------------------|---|---|---|---|---|
| 5 - Severe | 5 | 10 | 15 | 20 | 25 |
| 4 - Major | 4 | 8 | 12 | 16 | 20 |
| 3 - Moderate | 3 | 6 | 9 | 12 | 15 |
| 2 - Minor | 2 | 4 | 6 | 8 | 10 |
| 1 - Negligible | 1 | 2 | 3 | 4 | 5 |

### 5.2 Risk Distribution

| Level | Count | Threats |
|-------|-------|---------|
| Critical (20-25) | 1 | T8 |
| High (12-19) | 2 | T1, T5 |
| Medium (5-11) | 5 | T2, T3, T4, T6, T7 |
| Low (1-4) | 0 | None |

## 6. Threat-to-Control Mapping

| Threat | Control | Location | Status |
|--------|---------|----------|--------|
| T2 - Benefits Access | RBAC | app/routes/benefits.js | ✅ Applied |
| T3 - Stored XSS | URL validation | app/routes/profile.js | ✅ Applied |
| T5 - IDOR | Session-based auth | app/routes/allocations.js | ✅ Applied |
| T8 - eval Injection | Numeric validation | app/routes/contributions.js | ✅ Applied |
| T1 - Weak Auth | Rate limiting | Middleware | ⏳ Planned |
| T4 - No Audit Logs | Audit logging | Middleware | ⏳ Planned |
| T6 - Error Disclosure | Custom errors | app/routes/error.js | ⏳ Planned |
| T7 - DoS | Rate limiting | Middleware | ⏳ Planned |

## 7. OWASP Top 10:2025 Mapping

| OWASP Category | Threats |
|----------------|---------|
| A01: Broken Access Control | T2, T5 |
| A02: Security Misconfiguration | T6, T7 |
| A05: Injection | T3, T8 |
| A07: Authentication Failures | T1 |
| A09: Logging Failures | T4 |

## 8. Evidence References

### 8.1 Exploit Evidence (Before Fix)

| Threat | Evidence | File |
|--------|----------|------|
| T2 | Benefits exploit | EVID-17-vuln5-benefits-access.png |
| T3 | XSS exploit | EVID-14-vuln2-xss-exploit.png |
| T5 | IDOR exploit | EVID-15-vuln3-idor-exploit.png |
| T8 | eval exploit | EVID-13-vuln1-eval-exploit.png |

### 8.2 Fix Evidence (After Fix)

| Threat | Evidence | File |
|--------|----------|------|
| T2 | Benefits fixed | EVID-21-vuln4-fix.png |
| T3 | XSS fixed | EVID-19-vuln2-fix.png |
| T5 | IDOR fixed | EVID-20-vuln3-fix.png |
| T8 | eval fixed | EVID-18-vuln1-fix.png |

### 8.3 Pipeline Evidence

| Evidence | File | What It Shows |
|----------|------|---------------|
| EVID-23 | pipeline/EVID-23-pipeline-basic.png | Basic pipeline (3 jobs) |
| EVID-24 | pipeline/EVID-24-pipeline-red-run.png | Pipeline FAILED (blocking gate) |
| EVID-25 | pipeline/EVID-25-pipeline-green-run.png | Pipeline PASSED |

## 9. Pipeline Integration

The threat model is integrated with the CI/CD pipeline:

| Pipeline Gate | Related Threats |
|---------------|-----------------|
| SAST (Semgrep) | T3, T8 (injection) |
| Dependency Scan | Supply chain |
| Secrets Scan | Credential exposure |
| Container Scan | Image vulnerabilities |

*Blocking Gate:* SAST (Semgrep) detects eval injection and blocks the pipeline.

## 10. Conclusion

The STRIDE threat modeling exercise identified eight threats across the PensionGuard application. The four highest-priority threats (T2, T3, T5, T8) have been successfully remediated through secure coding fixes, verified by exploit-fix-retest evidence.

The remaining four threats (T1, T4, T6, T7) will be addressed through pipeline controls and future improvements.

This demonstrates the value of threat modeling in identifying security risks early and prioritizing remediation efforts.