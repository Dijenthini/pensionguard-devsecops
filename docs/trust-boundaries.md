# PensionGuard - Trust Boundaries
# Date: 2026-09-07
 
## Trust Boundary 1: User Browser → Web Application
- **Description:** User input crosses from untrusted browser into trusted web app
- **What crosses:** HTTP requests (login, profile updates, allocations, contributions)
- **Trust Level Change:** Untrusted → Semi-trusted
- **Security Risk:** User can submit malicious input (XSS, injection)
- **Required Controls:**
  - Input validation (allow-list approach)
  - Output encoding
  - Authentication checks
  - Session validation
- **Control Location:** `routes/*.js`, middleware
 
---
 
## Trust Boundary 2: Web Application → MongoDB
- **Description:** Database queries cross from web app to database
- **What crosses:** MongoDB queries (find, update, insert, delete)
- **Trust Level Change:** Semi-trusted → Trusted
- **Security Risk:** Query injection (NoSQL injection)
- **Required Controls:**
  - Parameterized queries
  - Input sanitization
  - Query validation
- **Control Location:** `data/*-dao.js`
 
---
 
## Trust Boundary 3: Admin → Regular User
- **Description:** Privilege separation between admin and regular users
- **What crosses:** Access to admin-only pages and functions
- **Trust Level Change:** Regular user → Admin user
- **Security Risk:** Users accessing functions they shouldn't
- **Required Controls:**
  - Role-Based Access Control (RBAC)
  - Server-side authorization checks
  - Session-based role validation
- **Control Location:** `routes/benefits.js`, `routes/admin/*.js`
 
---
 
## Summary Table
 
| Boundary | From | To | Main Risk | Key Control |
|----------|------|-----|-----------|-------------|
| 1 | Browser | Web App | Malicious input | Input validation + Encoding |
| 2 | Web App | MongoDB | Query injection | Parameterized queries |
| 3 | Regular User | Admin | Privilege escalation | RBAC + Authorization |