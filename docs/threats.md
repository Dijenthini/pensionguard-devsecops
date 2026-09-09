# STRIDE Threat Model - NodeGoat
# Date: 2026-09-07
# Created by: Member 2
 
## Vulnerability Evidence Mapping

| Threat | Vulnerability File | Evidence Screenshot |
|--------|-------------------|---------------------|
| T2 - NoSQL Injection | data/allocations-dao.js | EVID-16 |
| T3 - Stored XSS | routes/profile.js | EVID-14 |
| T5 - IDOR | routes/allocations.js | EVID-15 |
| T8 - Broken Access Control | routes/benefits.js | EVID-17 |

## STRIDE Categories
- **S** - Spoofing (pretending to be someone else)
- **T** - Tampering (modifying data)
- **R** - Repudiation (denying actions)
- **I** - Information Disclosure (leaking data)
- **D** - Denial of Service (crashing system)
- **E** - Elevation of Privilege (getting higher access)
 
---
 
## Threat #1: Weak Authentication (Spoofing)
- **STRIDE Category:** S (Spoofing)
- **Scenario:** Attacker brute-forces weak passwords to gain unauthorized access
- **Attacker:** External attacker with no account
- **Entry Point:** Login page (`/login`)
- **Affected Component:** Authentication system (`routes/auth.js`)
- **Protected Asset:** User accounts, personal data, financial data
- **Existing Weakness:** No account lockout, weak password policy
- **Proposed Control:** Strong password policy, rate limiting, MFA
- **Control Location:** `routes/auth.js` (Member 3's fix)
- **Likelihood:** 3 (Possible)
- **Impact:** 4 (Major)
- **Risk Score:** 12 (HIGH)
 
---
 
## Threat #2: NoSQL Injection (Tampering)
- **STRIDE Category:** T (Tampering)
- **Scenario:** Attacker injects malicious MongoDB query to bypass authentication or access data
- **Attacker:** External attacker or authenticated user
- **Entry Point:** Login form, allocations API
- **Affected Component:** Database queries (`data/allocations-dao.js`)
- **Protected Asset:** All database data
- **Existing Weakness:** User input inserted directly into `$where` queries
- **Proposed Control:** Parameterized queries, input validation, sanitization
- **Control Location:** `data/allocations-dao.js` (Member 3's fix)
- **Likelihood:** 4 (Likely)
- **Impact:** 5 (Severe)
- **Risk Score:** 20 (HIGH)
 - **Evidence:** EVID-16-vuln2-nosql-exploit.png (Exploit demonstrated by Member 3)
---
 
## Threat #3: Stored XSS (Tampering)
- **STRIDE Category:** T (Tampering)
- **Scenario:** Attacker stores malicious JavaScript in profile that executes in other users' browsers
- **Attacker:** Authenticated user
- **Entry Point:** Profile update form (`/profile`)
- **Affected Component:** User profiles, data rendering (`routes/profile.js`)
- **Protected Asset:** Session cookies, user data
- **Existing Weakness:** No output encoding for stored data
- **Proposed Control:** Context-aware output encoding, sanitization
- **Control Location:** `routes/profile.js` (Member 3's fix)
- **Likelihood:** 3 (Possible)
- **Impact:** 3 (Moderate)
- **Risk Score:** 9 (MEDIUM)
 - **Evidence:** EVID-14-vuln2-xss-exploit.png (Exploit demonstrated by Member 3)
---
 
## Threat #4: No Audit Logs (Repudiation)
- **STRIDE Category:** R (Repudiation)
- **Scenario:** Admin denies changing benefit start dates because there's no log
- **Attacker:** Malicious admin or insider
- **Entry Point:** Benefits management (`/benefits`)
- **Affected Component:** Audit logging system
- **Protected Asset:** Accountability and traceability
- **Existing Weakness:** No security logging
- **Proposed Control:** Security logging for critical actions
- **Control Location:** Pipeline/backend (`app.js`)
- **Likelihood:** 3 (Possible)
- **Impact:** 2 (Minor)
- **Risk Score:** 6 (MEDIUM)
 
---
 
## Threat #5: Insecure Direct Object Reference (Information Disclosure)
- **STRIDE Category:** I (Information Disclosure)
- **Scenario:** User changes URL parameter to access another user's allocations
- **Attacker:** Authenticated user
- **Entry Point:** `/allocations/:userId` URL
- **Affected Component:** Data access (`routes/allocations.js`)
- **Protected Asset:** Other users' financial data
- **Existing Weakness:** No ownership check
- **Proposed Control:** Server-side authorization check (user ID from session)
- **Control Location:** `routes/allocations.js` (Member 3's fix)
- **Likelihood:** 4 (Likely)
- **Impact:** 4 (Major)
- **Risk Score:** 16 (HIGH)
 *Evidence:* EVID-15-vuln3-idor-exploit.png (Exploit demonstrated by Member 3)
---
 
## Threat #6: Error Message Disclosure (Information Disclosure)
- **STRIDE Category:** I (Information Disclosure)
- **Scenario:** Application reveals system details in error messages
- **Attacker:** External attacker
- **Entry Point:** Any page that throws an error
- **Affected Component:** Error handling (`routes/error.js`)
- **Protected Asset:** Internal system information
- **Existing Weakness:** Verbose error messages
- **Proposed Control:** Custom error pages, safe error handling
- **Control Location:** `routes/error.js`
- **Likelihood:** 2 (Unlikely)
- **Impact:** 2 (Minor)
- **Risk Score:** 4 (MEDIUM)
 
---
 
## Threat #7: Denial of Service (Resource Exhaustion)
- **STRIDE Category:** D (Denial of Service)
- **Scenario:** Attacker sends many requests to slow down or crash the server
- **Attacker:** External attacker
- **Entry Point:** Any API endpoint
- **Affected Component:** Server resources
- **Protected Asset:** Application availability
- **Existing Weakness:** No rate limiting
- **Proposed Control:** Rate limiting, request throttling
- **Control Location:** Middleware (`app.js`)
- **Likelihood:** 2 (Unlikely)
- **Impact:** 3 (Moderate)
- **Risk Score:** 6 (MEDIUM)
 
---
 
## Threat #8: Broken Access Control (Elevation of Privilege)
- **STRIDE Category:** E (Elevation of Privilege)
- **Scenario:** Regular user accesses admin-only benefits page via direct URL
- **Attacker:** Authenticated regular user
- **Entry Point:** `/benefits` URL
- **Affected Component:** Authorization system (`routes/benefits.js`)
- **Protected Asset:** Admin functions
- **Existing Weakness:** No role check on server
- **Proposed Control:** RBAC, server-side authorization
- **Control Location:** `routes/benefits.js`
- **Likelihood:** 3 (Possible)
- **Impact:** 4 (Major)
- **Risk Score:** 12 (HIGH)
- **Evidence:** EVID-17-vuln5-benefits-access.png (Exploit demonstrated by Member 3)