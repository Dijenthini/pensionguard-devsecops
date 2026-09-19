# Secure Coding Section - Report
# Date: 2026-09-19
# Member 3 - Secure Coding Lead

## 1. Introduction

Four vulnerabilities were identified and remediated in the PensionGuard application (OWASP NodeGoat). Each vulnerability was demonstrated working before the fix and verified blocked after the fix, with all evidence captured.

## 2. Vulnerability 1: Server-Side JavaScript Injection (eval)

### 2.1 Root Cause
The application used `eval()` to process user-supplied contribution values:
```javascript
const preTax = eval(req.body.preTax);
const afterTax = eval(req.body.afterTax);
const roth = eval(req.body.roth);
```
This allowed attackers to execute arbitrary JavaScript code on the server.

### 2.2 Exploit
**Payload:** `require('child_process').execSync('echo HACKED')`
**Result:** Command execution on server
**Evidence:** EVID-13-vuln1-eval-exploit.png

### 2.3 Fix
Replaced `eval()` with safe numeric parsing:
```javascript
function safeParseNumber(value) {
    const num = parseFloat(value);
    if (isNaN(num) || !isFinite(num)) return null;
    return num;
}
const preTax = safeParseNumber(req.body.preTax);
const afterTax = safeParseNumber(req.body.afterTax);
const roth = safeParseNumber(req.body.roth);
```

### 2.4 Verification
- ✅ Exploit blocked: "Invalid contribution percentages"
- ✅ Evidence: EVID-18-vuln1-fix.png

---

## 3. Vulnerability 2: Stored XSS

### 3.1 Root Cause
User-supplied website URL was encoded for HTML context but used in a URL attribute context.

### 3.2 Exploit
**Payload:** `javascript:alert('XSS')`
**Result:** Script execution when profile viewed
**Evidence:** EVID-14-vuln2-xss-exploit.png

### 3.3 Fix
Added URL scheme validation:
```javascript
function validateUrl(url) {
    try {
        const parsed = new URL(url);
        return parsed.protocol === 'http:' || parsed.protocol === 'https:';
    } catch (e) {
        return false;
    }
}
if (req.body.website && !validateUrl(req.body.website)) {
    req.body.website = '';
}
```

### 3.4 Verification
- ✅ Exploit blocked: XSS payload rejected
- ✅ Evidence: EVID-19-vuln2-fix.png

---

## 4. Vulnerability 3: IDOR

### 4.1 Root Cause
User ID was taken from URL parameter instead of session:
```javascript
const { userId } = req.params;  // Vulnerable
```

### 4.2 Exploit
**URL:** `/allocations/2` (while logged in as user1)
**Result:** Access to user2's data
**Evidence:** EVID-15-vuln3-idor-exploit.png

### 4.3 Fix
Changed to use session userId:
```javascript
const userId = req.session.userId;  // Secure
```

### 4.4 Verification
- ✅ Exploit blocked: Cannot access other user's data
- ✅ Evidence: EVID-20-vuln3-fix.png

---

## 5. Vulnerability 4: Benefits Access

### 5.1 Root Cause
No server-side admin check on benefits page:
```javascript
this.displayBenefits = (req, res, next) => {
    // No authorization check
    benefitsDAO.getAllNonAdminUsers(...);
};
```

### 5.2 Exploit
**URL:** `/benefits` (while logged in as user1)
**Result:** Access to admin-only benefits data
**Evidence:** EVID-17-vuln5-benefits-access.png

### 5.3 Fix
Added server-side RBAC check:
```javascript
if (!req.session.user || req.session.user.username !== 'admin') {
    return res.status(403).send('Access denied. Admin only.');
}
```

### 5.4 Verification
- ✅ Exploit blocked: "Access denied. Admin only."
- ✅ Evidence: EVID-21-vuln4-fix.png

---

## 6. SAST Comparison

| Scan | Findings |
|------|----------|
| Baseline (vulnerable) | 6 |
| Secured (fixed) | 0 |

The Semgrep SAST scan detected 6 vulnerabilities in the baseline version (all related to eval injection). After applying secure coding fixes, the scan reported 0 findings, proving the remediation was successful.

**Evidence:** EVID-22-sast-after.png

---

## 7. Regression Tests

Regression tests were added to prevent reintroduction:
- `test/security/regression.test.js`

| Test | Purpose |
|------|---------|
| eval injection prevention | Ensures eval is not used |
| XSS prevention | Ensures URL validation works |
| IDOR prevention | Ensures session auth is used |
| Benefits access | Ensures RBAC is enforced |

---

## 8. Summary

| Vulnerability | Status | Exploit Evidence | Fix Evidence |
|---------------|--------|------------------|--------------|
| eval Injection | ✅ Fixed | EVID-13 | EVID-18 |
| Stored XSS | ✅ Fixed | EVID-14 | EVID-19 |
| IDOR | ✅ Fixed | EVID-15 | EVID-20 |
| Benefits Access | ✅ Fixed | EVID-17 | EVID-21 |

All 4 vulnerabilities have been successfully remediated and verified. SAST findings dropped from 6 to 0.

## 9. Cross-Reference with Threat Model

| Vulnerability | Related Threat | Threat Status |
|---------------|----------------|---------------|
| eval Injection | T8 - Server-Side JS Injection | ✅ Fixed |
| Stored XSS | T3 - Stored XSS | ✅ Fixed |
| IDOR | T5 - IDOR (Allocations) | ✅ Fixed |
| Benefits Access | T2 - Benefits Access | ✅ Fixed |

## 10. Evidence Mapping

| Vulnerability | Exploit Evidence | Fix Evidence |
|---------------|------------------|--------------|
| eval Injection | EVID-13-vuln1-eval-exploit.png | EVID-18-vuln1-fix.png |
| Stored XSS | EVID-14-vuln2-xss-exploit.png | EVID-19-vuln2-fix.png |
| IDOR | EVID-15-vuln3-idor-exploit.png | EVID-20-vuln3-fix.png |
| Benefits Access | EVID-17-vuln5-benefits-access.png | EVID-21-vuln4-fix.png |

## 11. SAST Evidence

| Scan | Findings | Evidence |
|------|----------|----------|
| Baseline (vulnerable) | 6 | baseline_only_fixed.txt |
| Secured (fixed) | 0 | secured_only_fixed.txt |
| Comparison | 6 → 0 | EVID-22-sast-after.png |