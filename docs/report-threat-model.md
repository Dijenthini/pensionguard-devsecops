# Threat Model Section - Draft
# Date: 2026-09-09

## Introduction

The PensionGuard application (OWASP NodeGoat) was analyzed using the STRIDE threat modeling methodology. Eight threats were identified across six STRIDE categories.

## Threat Model Summary

| Category | Number of Threats | Examples |
|----------|-------------------|----------|
| Spoofing (S) | 1 | Weak Authentication (T1) |
| Tampering (T) | 1 | Stored XSS (T3) |
| Repudiation (R) | 1 | No Audit Logs (T4) |
| Information Disclosure (I) | 3 | IDOR (T5, T8), Error Disclosure (T6) |
| Denial of Service (D) | 1 | Resource Exhaustion (T7) |
| Elevation of Privilege (E) | 1 | Broken Access Control - Benefits (T2) |

## Risk Assessment Results

| Risk Level | Number of Threats |
|------------|-------------------|
| Critical (20-25) | 0 |
| High (12-19) | 3 |
| Medium (5-11) | 4 |
| Low (1-4) | 1 |

## Threat-to-Control Mapping

| Threat | Control | Implementation Location |
|--------|---------|------------------------|
| Broken Access Control (Benefits) | RBAC | routes/benefits.js |
| Stored XSS | Output encoding | routes/profile.js |
| IDOR | Session-based authorization | routes/allocations.js |
| Weak Authentication | Rate limiting | routes/auth.js |
| Error Disclosure | Custom error handling | routes/error.js |

## OWASP Top 10:2025 Mapping

The identified threats were mapped to OWASP Top 10:2025 categories:

| OWASP Category | Threats | Controls |
|----------------|---------|----------|
| A01: Broken Access Control | T2, T5, T8 | RBAC, session auth |
| A03: Injection | T3 | Output encoding |
| A05: Security Misconfiguration | T6, T7 | Error handling, rate limiting |
| A07: Identification Failures | T1 | Rate limiting, strong passwords |
| A09: Security Logging Failures | T4 | Audit logging |

---

## Risk Treatment Summary

| Action | Threats |
|--------|---------|
| Mitigated (Applied) | T2, T3, T5, T8 |
| Planned (Pending) | T1, T4, T6, T7 |
| Accepted | None |

---

## Threat Model Conclusion

The threat modeling exercise identified eight threats across six STRIDE categories and five OWASP Top 10:2025 categories. The highest priority threats (Critical and High) have been addressed through secure coding fixes (Member 3). Remaining Medium-level threats will be addressed through pipeline controls.

Key improvements:
- ✅ RBAC implemented for benefits page
- ✅ Output encoding for XSS prevention
- ✅ Session-based authorization for allocations
- ⏳ Rate limiting (planned)
- ⏳ Audit logging (planned)

## Conclusion

The threat modeling exercise identified eight threats requiring mitigation. The highest priority threats (Critical and High) have been addressed through secure coding fixes implemented by Member 3. Remaining Medium-level threats will be addressed through pipeline controls and future improvements.