# System Overview - PensionGuard
# Date: 2026-09-08

## Application
- **Name:** OWASP NodeGoat
- **Working Title:** PensionGuard
- **Purpose:** Retirement savings management
- **Tech Stack:** Node.js, Express, MongoDB

## Components
| Component | Technology | Port |
|-----------|------------|------|
| User Browser | HTML/CSS/JS | - |
| Web App | Node.js/Express | 4000 |
| Database | MongoDB 4.4 | 27017 |

## Security Features
- Non-root container
- Database isolated
- Resource limits
- CI/CD security scanning

## Test Accounts
| Username | Password | Role |
|----------|----------|------|
| admin | Admin_123 | Admin |
| user1 | User1_123 | User |
| user2 | User2_123 | User |

## Security Controls Summary

| Control Type | Control | Status |
|--------------|---------|--------|
| Preventive | Input Validation | ✅ Applied |
| Preventive | Authentication | ✅ Applied |
| Preventive | Authorization (RBAC) | ✅ Applied |
| Preventive | Output Encoding | ✅ Applied |
| Preventive | Non-Root Container | ✅ Applied |
| Preventive | Database Isolation | ✅ Applied |
| Detective | SAST Scanning | ✅ Applied |
| Detective | SCA Scanning | ✅ Applied |
| Detective | Secrets Scanning | ✅ Applied |
| Detective | Container Scanning | ✅ Applied |
| Detective | Audit Logging | ⏳ Planned |
| Corrective | Error Handling | ✅ Applied |
| Deterrent | Rate Limiting | ⏳ Planned |

## Key Security Features

1. **Input Validation:** All user input validated using allow-list approach
2. **Authentication:** Session-based authentication with secure cookies
3. **Authorization:** RBAC with server-side checks
4. **Output Encoding:** Context-aware encoding to prevent XSS
5. **Container Security:** Non-root user, resource limits, minimal image
6. **Database Security:** MongoDB not exposed to host
7. **CI/CD Security:** 4 automated security gates (SAST, SCA, secrets, container)
8. **Error Handling:** Custom error pages, no stack traces

## Security Metrics

| Metric | Value |
|--------|-------|
| SAST Findings (Before) | [Insert count from EVID-11] |
| SAST Findings (After) | [Insert count after fixes] |
| Vulnerabilities Fixed | 4 |
| Security Gates in Pipeline | 4 |
| Secrets Exposed | 0 |