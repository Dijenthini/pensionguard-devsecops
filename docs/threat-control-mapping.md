# Threat-to-Control Mapping - NodeGoat
# Date: 2026-09-09
# Created by Member 2

| Threat | Control | Location |
|--------|---------|----------|
| T1 - Weak Authentication | Rate limiting, strong passwords | routes/auth.js |
| T2 - Broken Access Control (Benefits) | RBAC, server-side authorization | routes/benefits.js |
| T3 - Stored XSS | Output encoding | routes/profile.js |
| T4 - No Audit Logs | Security logging | Middleware |
| T5 - IDOR (Allocations) | Session-based authorization | routes/allocations.js |
| T6 - Error Disclosure | Custom error handling | routes/error.js |
| T7 - DoS | Rate limiting | Middleware |
| T8 - IDOR (Allocations) | Session-based authorization | routes/allocations.js |
 
## Control Types
 
| Type | Count | Examples |
|------|-------|----------|
| Preventive (before attack) | 5 | Input validation, Authentication, Authorization |
| Detective (during attack) | 1 | Audit logging |
| Corrective (after attack) | 1 | Error handling |
| Deterrent (discourage attack) | 1 | Rate limiting |