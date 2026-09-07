# Threat-to-Control Mapping - NodeGoat
# Date: 2026-09-07
# Created by: Member 2
 
| Threat ID | Threat Name | Control | Control Location | Implementation Status |
|-----------|-------------|---------|------------------|----------------------|
| T1 | Weak Authentication | Strong password policy + Rate limiting | `routes/auth.js` | Member 3's fix |
| T2 | NoSQL Injection | Parameterized queries + Input validation | `data/allocations-dao.js` | Member 3's fix |
| T3 | Stored XSS | Output encoding + Sanitization | `routes/profile.js` | Member 3's fix |
| T4 | No Audit Logs | Security logging | `routes/*.js` + Middleware | Pipeline |
| T5 | IDOR | Server-side authorization (session ID) | `routes/allocations.js` | Member 3's fix |
| T6 | Error Disclosure | Custom error handling | `routes/error.js` | Code |
| T7 | Denial of Service | Rate limiting | Middleware (`app.js`) | Pipeline |
| T8 | Broken Access Control | RBAC + Authorization checks | `routes/benefits.js` | Code |
 
## Control Types
 
| Type | Count | Examples |
|------|-------|----------|
| Preventive (before attack) | 5 | Input validation, Authentication, Authorization |
| Detective (during attack) | 1 | Audit logging |
| Corrective (after attack) | 1 | Error handling |
| Deterrent (discourage attack) | 1 | Rate limiting |