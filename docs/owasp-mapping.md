# OWASP Top 10:2025 Mapping - PensionGuard
# Date: 2026-09-19
# Member 2 - Threat Modeling Lead

## OWASP Top 10:2025 Categories

| Rank | Category |
|------|----------|
| A01 | Broken Access Control |
| A02 | Security Misconfiguration |
| A03 | Software Supply Chain Failures |
| A04 | Cryptographic Failures |
| A05 | Injection |
| A06 | Insecure Design |
| A07 | Authentication Failures |
| A08 | Software and Data Integrity Failures |
| A09 | Logging and Monitoring Failures |
| A10 | Server-Side Request Forgery |

## Threat-to-OWASP Mapping

| Threat ID | Threat Name | OWASP Category | Justification |
|-----------|-------------|----------------|---------------|
| T1 | Weak Authentication | A07: Authentication Failures | No rate limiting, weak password policy |
| T2 | Broken Access Control (Benefits) | A01: Broken Access Control | Regular users access admin-only page |
| T3 | Stored XSS | A05: Injection | Malicious script stored and executed |
| T4 | No Audit Logs | A09: Logging Failures | No logs for admin actions |
| T5 | IDOR (Allocations) | A01: Broken Access Control | Access other users' data via URL |
| T6 | Error Disclosure | A02: Security Misconfiguration | Verbose errors expose system details |
| T7 | DoS | A02: Security Misconfiguration | No rate limiting, resource exhaustion |
| T8 | IDOR | A01: Broken Access Control | Access other users' data via URL |

## Summary by OWASP Category

| OWASP Category | Count | Threats |
|----------------|-------|---------|
| A01: Broken Access Control | 3 | T2, T5, T8 |
| A02: Security Misconfiguration | 2 | T6, T7 |
| A05: Injection | 1 | T3 |
| A07: Authentication Failures | 1 | T1 |
| A09: Logging Failures | 1 | T4 |

## Controls Applied per OWASP Category

| OWASP Category | Controls |
|----------------|----------|
| A01: Broken Access Control | RBAC, session-based auth, server-side checks |
| A02: Security Misconfiguration | Custom error handling, rate limiting |
| A05: Injection | Output encoding, input validation |
| A07: Authentication Failures | Rate limiting, strong passwords |
| A09: Logging Failures | Audit logging (planned) |