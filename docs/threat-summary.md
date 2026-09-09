# Threat Summary Table - PensionGuard
# Date: 2026-09-08

| Threat ID | Threat Name | STRIDE | Likelihood | Impact | Score | Level | Evidence |
|-----------|-------------|--------|------------|--------|-------|-------|----------|
| T1 | Weak Authentication | S | 3 | 4 | 12 | HIGH | Pending |
| T2 | NoSQL Injection | T | 4 | 5 | 20 | CRITICAL | EVID-16 |
| T3 | Stored XSS | T | 3 | 3 | 9 | MEDIUM | EVID-14 |
| T4 | No Audit Logs | R | 3 | 2 | 6 | MEDIUM | Pending |
| T5 | IDOR | I | 4 | 4 | 16 | HIGH | EVID-15 |
| T6 | Error Disclosure | I | 2 | 2 | 4 | MEDIUM | Pending |
| T7 | DoS | D | 2 | 3 | 6 | MEDIUM | Pending |
| T8 | Broken Access Control | E | 3 | 4 | 12 | HIGH | EVID-17 |

## Summary
- *Critical:* 1 threat (T2 - NoSQL Injection)
- *High:* 3 threats (T1, T5, T8)
- *Medium:* 4 threats (T3, T4, T6, T7)
- *Low:* 0 threats

## Controls Summary
| Control Type | Threats Covered |
|--------------|-----------------|
| Input Validation | T1, T2, T3, T5, T6, T8 |
| Authorization | T1, T5, T8 |
| Logging | T4 |
| Rate Limiting | T1, T7 |
| Output Encoding | T3 |