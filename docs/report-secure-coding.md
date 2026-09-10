# Secure Coding Section - Draft
# Date: 2026-09-10
# Member 3 - Secure Coding Lead

## 1. Introduction

Four vulnerabilities were identified and remediated in the PensionGuard application (OWASP NodeGoat). Each vulnerability was demonstrated working before the fix and verified blocked after the fix.

---

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
Payload: `require('child_process').execSync('echo HACKED')`
Result: Command execution on server

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
- ✅ Evidence: EVID-18

---

## 3. Vulnerability 2: Stored XSS

### 3.1 Root Cause
User-supplied website URL was encoded for HTML context but used in a URL attribute context.

### 3.2 Exploit
Payload: `javascript:alert('XSS')`
Result: Script execution when profile viewed

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
- ✅ Evidence: EVID-19

---

## 4. Vulnerability 3: IDOR

### 4.1 Root Cause
User ID was taken from URL parameter instead of session:
```javascript
const { userId } = req.params;  // Vulnerable
```

### 4.2 Exploit
URL: `/allocations/2` (while logged in as user1)
Result: Access to user2's data

### 4.3 Fix
Changed to use session userId:
```javascript
const userId = req.session.userId;  // Secure
```

### 4.4 Verification
- ✅ Exploit blocked: Cannot access other user's data
- ✅ Evidence: EVID-20

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
URL: `/benefits` (while logged in as user1)
Result: Access to admin-only benefits data

### 5.3 Fix
Added server-side RBAC check:
```javascript
if (!req.session.user || req.session.user.username !== 'admin') {
    return res.status(403).send('Access denied. Admin only.');
}
```

### 5.4 Verification
- ✅ Exploit blocked: "Access denied. Admin only."
- ✅ Evidence: EVID-21

---

## 6. SAST Comparison

| Scan | Findings |
|------|----------|
| Baseline (vulnerable) | 28 |
| Secured (fixed) | 28 |

Note: SAST did not detect the 4 vulnerabilities. Manual testing proved the fixes.

---

## 7. Summary

| Vulnerability | Status | Evidence |
|---------------|--------|----------|
| eval Injection | ✅ Fixed | EVID-18 |
| Stored XSS | ✅ Fixed | EVID-19 |
| IDOR | ✅ Fixed | EVID-20 |
| Benefits Access | ✅ Fixed | EVID-21 |

All 4 vulnerabilities have been successfully remediated.