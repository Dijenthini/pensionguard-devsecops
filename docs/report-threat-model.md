# Threat Model Section - Draft
# Date: 2026-09-08

## Introduction

The PensionGuard application (OWASP NodeGoat) was analyzed using the STRIDE threat modeling methodology to identify security risks. Eight threats were identified across six STRIDE categories.

## Threat Model Summary

| Category | Number of Threats | Examples |
|----------|-------------------|----------|
| Spoofing (S) | 1 | Weak Authentication (T1) |
| Tampering (T) | 2 | NoSQL Injection (T2), Stored XSS (T3) |
| Repudiation (R) | 1 | No Audit Logs (T4) |
| Information Disclosure (I) | 2 | IDOR (T5), Error Disclosure (T6) |
| Denial of Service (D) | 1 | Resource Exhaustion (T7) |
| Elevation of Privilege (E) | 1 | Broken Access Control (T8) |

## Risk Assessment Results

| Risk Level | Number of Threats | Actions Required |
|------------|-------------------|------------------|
| Critical (20-25) | 1 | Immediate action required |
| High (12-19) | 3 | Priority action required |
| Medium (5-11) | 4 | Action planned |
| Low (1-4) | 0 | Acceptable risk |

## Threat-to-Control Mapping

| Threat | Control | Implementation Location |
|--------|---------|------------------------|
| NoSQL Injection | Parameterized queries | data/allocations-dao.js |
| Stored XSS | Output encoding | routes/profile.js |
| IDOR | Session-based authorization | routes/allocations.js |
| Broken Access Control | RBAC | routes/benefits.js |
| Weak Authentication | Rate limiting | routes/auth.js |
| Error Disclosure | Custom error handling | routes/error.js |

## Conclusion

The threat modeling exercise identified eight threats requiring mitigation. The highest priority threats (Critical and High) have been addressed through secure coding fixes implemented by Member 3. Remaining Medium-level threats will be addressed through pipeline controls and future improvements.