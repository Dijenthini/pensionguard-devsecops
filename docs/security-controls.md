# Security Controls - PensionGuard
# Date: 2026-09-09

## Control Categories

| Category | Description | Examples |
|----------|-------------|----------|
| **Preventive** | Stop attacks before they happen | Input validation, authentication, RBAC |
| **Detective** | Identify attacks in progress | Audit logging, monitoring |
| **Corrective** | Respond to and recover from attacks | Error handling, backups |
| **Deterrent** | Discourage attackers | Rate limiting, security warnings |

---

## Preventive Controls

| Control | Implementation | Location |
|---------|----------------|----------|
| Input Validation | Allow-list validation | `routes/*.js` |
| Authentication | Session-based login | `routes/auth.js` |
| Authorization | RBAC, server-side checks | `routes/benefits.js`, `routes/allocations.js` |
| Output Encoding | Context-aware encoding | `routes/profile.js` |
| Parameterized Queries | MongoDB queries with sanitization | `data/*-dao.js` |
| Non-Root User | `USER node` in Dockerfile | Dockerfile |
| Database Isolation | MongoDB not exposed to host | docker-compose.yml |

---

## Detective Controls

| Control | Implementation | Location |
|---------|----------------|----------|
| Audit Logging | Security event logging | Middleware (planned) |
| SAST Scanning | Semgrep in CI/CD | `.github/workflows/security.yml` |
| SCA Scanning | npm audit in CI/CD | `.github/workflows/security.yml` |
| Secrets Scanning | Gitleaks in CI/CD | `.github/workflows/security.yml` |
| Container Scanning | Trivy in CI/CD | `.github/workflows/security.yml` |

---

## Corrective Controls

| Control | Implementation | Location |
|---------|----------------|----------|
| Error Handling | Custom error pages | `routes/error.js` |
| Graceful Degradation | Fail secure, not expose details | `routes/*.js` |
| Backup/Restore | MongoDB data persistence | docker-compose.yml |

---

## Deterrent Controls

| Control | Implementation | Location |
|---------|----------------|----------|
| Rate Limiting | Request throttling | Middleware (planned) |
| Security Headers | CSP, X-Frame-Options | Middleware (planned) |

---

## Control Mapping to Threats

| Threat | Primary Control | Secondary Control |
|--------|-----------------|-------------------|
| T1 - Weak Authentication | Rate limiting | Strong password policy |
| T2 - Broken Access Control | RBAC | Server-side authorization |
| T3 - Stored XSS | Output encoding | Input validation |
| T4 - No Audit Logs | Audit logging | Monitoring |
| T5 - IDOR | Session-based auth | Ownership check |
| T6 - Error Disclosure | Custom error handling | Input validation |
| T7 - DoS | Rate limiting | Resource limits |
| T8 - IDOR | Session-based auth | Ownership check |

---

## Summary

| Control Type | Count | Implemented |
|--------------|-------|-------------|
| Preventive   | 7     |    ✅ 6    |
| Detective    | 4     |    ✅ 3    |
| Corrective   | 2     |    ✅ 2    |
| Deterrent    | 2     |     ⏳ 0    |