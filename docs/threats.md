# STRIDE Threat Model - NodeGoat
# Date: 2026-09-19
# Created by: Member 2

## Pipeline Status Integration

The threat model has been integrated with the CI/CD pipeline status:

| Threat | Status | Pipeline Gate |
|--------|--------|---------------|
| T1 - Weak Authentication | ⏳ Pending | Rate limiting (planned) |
| T2 - Benefits Access | ✅ Fixed | SAST, Container Scan |
| T3 - Stored XSS | ✅ Fixed | SAST |
| T4 - No Audit Logs | ⏳ Pending | Logging (planned) |
| T5 - IDOR | ✅ Fixed | SAST |
| T6 - Error Disclosure | ⏳ Pending | Custom errors (planned) |
| T7 - DoS | ⏳ Pending | Rate limiting (planned) |
| T8 - eval Injection | ✅ Fixed | SAST (blocking) |

## Pipeline Security Gates

| Gate | Purpose | Blocking? |
|------|---------|-----------|
| SAST (Semgrep) | Detect code vulnerabilities | ✅ Yes |
| Dependency Scan | Find vulnerable dependencies | No |
| Secrets Scan | Detect exposed secrets | No |
| Container Scan | Scan Docker image | No |

## Vulnerability Evidence Mapping

| Threat | Vulnerability File | Exploit Evidence | Fix Evidence |
|--------|-------------------|------------------|--------------|
| T1 - Weak Authentication | app/routes/auth.js | Pending | Pending |
| T2 - Broken Access Control (Benefits) | app/routes/benefits.js | EVID-17-vuln5-benefits-access.png | EVID-21-vuln4-fix.png |
| T3 - Stored XSS | app/routes/profile.js | EVID-14-vuln2-xss-exploit.png | EVID-19-vuln2-fix.png |
| T4 - No Audit Logs | Middleware | Pending | Pending |
| T5 - IDOR (Allocations) | app/routes/allocations.js | EVID-15-vuln3-idor-exploit.png | EVID-20-vuln3-fix.png |
| T6 - Error Disclosure | app/routes/error.js | Pending | Pending |
| T7 - DoS | Middleware | Pending | Pending |
| T8 - Server-Side JS Injection (eval) | app/routes/contributions.js | EVID-13-vuln1-eval-exploit.png | EVID-18-vuln1-fix.png |

## STRIDE Categories
- *S* - Spoofing (pretending to be someone else)
- *T* - Tampering (modifying data)
- *R* - Repudiation (denying actions)
- *I* - Information Disclosure (leaking data)
- *D* - Denial of Service (crashing system)
- *E* - Elevation of Privilege (getting higher access)

---

## Threat #1: Weak Authentication (Spoofing)
- *STRIDE Category:* S (Spoofing)
- *Scenario:* Attacker brute-forces weak passwords to gain unauthorized access
- *Attacker:* External attacker with no account
- *Entry Point:* Login page (/login)
- *Affected Component:* Authentication system (app/routes/auth.js)
- *Protected Asset:* User accounts, personal data, financial data
- *Existing Weakness:* No account lockout, weak password policy
- *Proposed Control:* Strong password policy, rate limiting, MFA
- *Control Location:* app/routes/auth.js
- *Likelihood:* 3 (Possible)
- *Impact:* 4 (Major)
- *Risk Score:* 12 (HIGH)
- *Evidence:* Pending
- *Status:* ⏳ PENDING

---

## Threat #2: Broken Access Control - Benefits Page (Elevation of Privilege)
- *STRIDE Category:* E (Elevation of Privilege)
- *Scenario:* A regular user accesses the admin-only Benefits page by directly typing /benefits in the URL
- *Attacker:* Authenticated regular user (user1, user2)
- *Entry Point:* /benefits URL
- *Affected Component:* app/routes/benefits.js
- *Asset at Risk:* Employee benefits data (start dates, employee IDs)
- *Weakness:* No server-side role check — only hidden from menu, not protected
- *Proposed Control:* Role-Based Access Control (RBAC), server-side authorization check
- *Control Location:* app/routes/benefits.js
- *Likelihood:* 3 (Possible)
- *Impact:* 3 (Moderate)
- *Risk Score:* 9 (MEDIUM)
- *Evidence:* EVID-17-vuln5-benefits-access.png (Exploit demonstrated by Member 3)
- *Status:* ✅ FIXED
- *Fix Evidence:* EVID-21-vuln4-fix.png

---

## Threat #3: Stored XSS (Tampering)
- *STRIDE Category:* T (Tampering)
- *Scenario:* Attacker stores malicious JavaScript in profile that executes in other users' browsers
- *Attacker:* Authenticated user
- *Entry Point:* Profile update form (/profile)
- *Affected Component:* User profiles, data rendering (app/routes/profile.js)
- *Protected Asset:* Session cookies, user data
- *Existing Weakness:* No output encoding for stored data
- *Proposed Control:* Context-aware output encoding, sanitization
- *Control Location:* app/routes/profile.js
- *Likelihood:* 3 (Possible)
- *Impact:* 3 (Moderate)
- *Risk Score:* 9 (MEDIUM)
- *Evidence:* EVID-14-vuln2-xss-exploit.png (Exploit demonstrated by Member 3)
- *Status:* ✅ FIXED
- *Fix Evidence:* EVID-19-vuln2-fix.png

---

## Threat #4: No Audit Logs (Repudiation)
- *STRIDE Category:* R (Repudiation)
- *Scenario:* Admin denies changing benefit start dates because there's no log
- *Attacker:* Malicious admin or insider
- *Entry Point:* Benefits management (/benefits)
- *Affected Component:* Audit logging system
- *Protected Asset:* Accountability and traceability
- *Existing Weakness:* No security logging
- *Proposed Control:* Security logging for critical actions
- *Control Location:* Pipeline/backend (app.js)
- *Likelihood:* 3 (Possible)
- *Impact:* 2 (Minor)
- *Risk Score:* 6 (MEDIUM)
- *Evidence:* Pending
- *Status:* ⏳ PENDING

---

## Threat #5: IDOR - Allocations Access (Information Disclosure)
- *STRIDE Category:* I (Information Disclosure)
- *Scenario:* User changes URL parameter to access another user's allocations
- *Attacker:* Authenticated user
- *Entry Point:* /allocations/:userId URL
- *Affected Component:* Data access (app/routes/allocations.js)
- *Protected Asset:* Other users' financial data
- *Existing Weakness:* No ownership check
- *Proposed Control:* Server-side authorization check (user ID from session)
- *Control Location:* app/routes/allocations.js
- *Likelihood:* 4 (Likely)
- *Impact:* 4 (Major)
- *Risk Score:* 16 (HIGH)
- *Evidence:* EVID-15-vuln3-idor-exploit.png (Exploit demonstrated by Member 3)
- *Status:* ✅ FIXED
- *Fix Evidence:* EVID-20-vuln3-fix.png

---

## Threat #6: Error Message Disclosure (Information Disclosure)
- *STRIDE Category:* I (Information Disclosure)
- *Scenario:* Application reveals system details in error messages
- *Attacker:* External attacker
- *Entry Point:* Any page that throws an error
- *Affected Component:* Error handling (app/routes/error.js)
- *Protected Asset:* Internal system information
- *Existing Weakness:* Verbose error messages
- *Proposed Control:* Custom error pages, safe error handling
- *Control Location:* app/routes/error.js
- *Likelihood:* 2 (Unlikely)
- *Impact:* 2 (Minor)
- *Risk Score:* 4 (MEDIUM)
- *Evidence:* Pending
- *Status:* ⏳ PENDING

---

## Threat #7: Denial of Service (Resource Exhaustion)
- *STRIDE Category:* D (Denial of Service)
- *Scenario:* Attacker sends many requests to slow down or crash the server
- *Attacker:* External attacker
- *Entry Point:* Any API endpoint
- *Affected Component:* Server resources
- *Protected Asset:* Application availability
- *Existing Weakness:* No rate limiting
- *Proposed Control:* Rate limiting, request throttling
- *Control Location:* Middleware (app.js)
- *Likelihood:* 2 (Unlikely)
- *Impact:* 3 (Moderate)
- *Risk Score:* 6 (MEDIUM)
- *Evidence:* Pending
- *Status:* ⏳ PENDING

---

## Threat #8: Server-Side JavaScript Injection - eval (Tampering)
- *STRIDE Category:* T/E (Tampering / Elevation of Privilege)
- *Scenario:* Attacker submits malicious JavaScript in the contribution field, which is executed by eval() on the server
- *Attacker:* Authenticated user
- *Entry Point:* /contributions form
- *Affected Component:* app/routes/contributions.js
- *Protected Asset:* Server, database, sensitive files
- *Existing Weakness:* Unsafe use of eval() on user input
- *Proposed Control:* Numeric parsing, input validation, remove eval()
- *Control Location:* app/routes/contributions.js
- *Likelihood:* 4 (Likely)
- *Impact:* 5 (Severe)
- *Risk Score:* 20 (CRITICAL)
- *Evidence:* EVID-13-vuln1-eval-exploit.png (Exploit demonstrated by Member 3)
- *Status:* ✅ FIXED
- *Fix Evidence:* EVID-18-vuln1-fix.png
---