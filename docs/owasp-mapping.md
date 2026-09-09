# OWASP Top 10:2025 Mapping - PensionGuard
# Date: 2026-09-09

## OWASP Top 10:2025 Categories

| Rank | Category | Description |
|------|----------|-------------|
| A01 | Broken Access Control | Access control failures |
| A02 | Cryptographic Failures | Weak cryptography |
| A03 | Injection | SQL/NoSQL/Command injection |
| A04 | Insecure Design | Design-level flaws |
| A05 | Security Misconfiguration | Misconfigured security settings |
| A06 | Vulnerable Components | Outdated dependencies |
| A07 | Identification Failures | Authentication weaknesses |
| A08 | Software/Data Integrity Failures | Supply chain attacks |
| A09 | Security Logging Failures | Missing audit logs |
| A10 | Server-Side Request Forgery | Unvalidated URLs |

---

## Threat-to-OWASP Mapping

| Threat ID | Threat Name | OWASP Category | Justification |
|-----------|-------------|----------------|---------------|
| T1 | Weak Authentication | A07: Identification Failures | Weak passwords, no MFA, no rate limiting |
| T2 | Broken Access Control (Benefits) | A01: Broken Access Control | Regular users access admin-only page |
| T3 | Stored XSS | A03: Injection | Malicious script stored and executed |
| T4 | No Audit Logs | A09: Security Logging Failures | No logs for admin actions |
| T5 | IDOR (Allocations) | A01: Broken Access Control | Access other users' data via URL |
| T6 | Error Disclosure | A05: Security Misconfiguration | Verbose error messages expose system details |
| T7 | Denial of Service | A05: Security Misconfiguration | No rate limiting, resource exhaustion |
| T8 | IDOR (Allocations) | A01: Broken Access Control | Access other users' data via URL |

---

## Summary

| OWASP Category | Number of Threats | Threats |
|----------------|-------------------|---------|
| A01: Broken Access Control | 3 | T2, T5, T8 |
| A03: Injection | 1 | T3 |
| A05: Security Misconfiguration | 2 | T6, T7 |
| A07: Identification Failures | 1 | T1 |
| A09: Security Logging Failures | 1 | T4 |

## Controls per OWASP Category

| OWASP Category | Controls Applied |
|----------------|------------------|
| A01: Broken Access Control | RBAC, session-based authorization, server-side checks |
| A03: Injection | Parameterized queries, output encoding, input validation |
| A05: Security Misconfiguration | Custom error handling, rate limiting |
| A07: Identification Failures | Rate limiting, strong password policy |
| A09: Security Logging Failures | Audit logging middleware |