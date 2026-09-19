# Secure Coding Summary - PensionGuard
# Date: 2026-09-19
# Member 3 - Secure Coding Lead

## 1. Overview

Four vulnerabilities were identified, exploited, fixed, and verified in the PensionGuard application. This document summarizes all secure coding work.

## 2. Vulnerability Fixes

### V1: Server-Side JavaScript Injection (eval)

| Aspect | Detail |
|--------|--------|
| **File** | `app/routes/contributions.js` |
| **Root Cause** | `eval()` used on user input |
| **Fix** | `safeParseNumber()` function |
| **Exploit Evidence** | EVID-13-vuln1-eval-exploit.png |
| **Fix Evidence** | EVID-18-vuln1-fix.png |
| **Status** | ✅ Fixed |

### V2: Stored XSS

| Aspect | Detail |
|--------|--------|
| **File** | `app/routes/profile.js` |
| **Root Cause** | Missing URL validation |
| **Fix** | `validateUrl()` function |
| **Exploit Evidence** | EVID-14-vuln2-xss-exploit.png |
| **Fix Evidence** | EVID-19-vuln2-fix.png |
| **Status** | ✅ Fixed |

### V3: IDOR (Allocations)

| Aspect | Detail |
|--------|--------|
| **File** | `app/routes/allocations.js` |
| **Root Cause** | `req.params.userId` used |
| **Fix** | `req.session.userId` used |
| **Exploit Evidence** | EVID-15-vuln3-idor-exploit.png |
| **Fix Evidence** | EVID-20-vuln3-fix.png |
| **Status** | ✅ Fixed |

### V4: Benefits Access

| Aspect | Detail |
|--------|--------|
| **File** | `app/routes/benefits.js` |
| **Root Cause** | No RBAC check |
| **Fix** | Admin username check |
| **Exploit Evidence** | EVID-17-vuln5-benefits-access.png |
| **Fix Evidence** | EVID-21-vuln4-fix.png |
| **Status** | ✅ Fixed |

## 3. SAST Results

| Scan | Findings | Evidence |
|------|----------|----------|
| Baseline (vulnerable) | 6 | baseline_only_fixed.txt |
| Secured (fixed) | 0 | secured_only_fixed.txt |
| **Improvement** | **6 → 0** | EVID-22-sast-after.png |

## 4. Regression Tests

| Test | File | Purpose |
|------|------|---------|
| eval prevention | `test/security/regression.test.js` | Prevent eval reintroduction |
| XSS prevention | `test/security/regression.test.js` | Prevent XSS reintroduction |
| IDOR prevention | `test/security/regression.test.js` | Prevent IDOR reintroduction |
| Benefits access | `test/security/regression.test.js` | Prevent RBAC bypass |

## 5. Summary

| Vulnerability | Status | Evidence Complete |
|---------------|--------|-------------------|
| V1 - eval Injection | ✅ Fixed | ✅ Yes |
| V2 - Stored XSS | ✅ Fixed | ✅ Yes |
| V3 - IDOR | ✅ Fixed | ✅ Yes |
| V4 - Benefits Access | ✅ Fixed | ✅ Yes |

## 6. Code Changes Summary

| File | Change | Lines |
|------|--------|-------|
| `app/routes/contributions.js` | Replaced `eval()` with `safeParseNumber()` | ~15 |
| `app/routes/profile.js` | Added `validateUrl()` | ~10 |
| `app/routes/allocations.js` | Changed to session userId | ~3 |
| `app/routes/benefits.js` | Added RBAC check | ~5 |

## 7. Verification

All fixes have been verified by:
1. Re-running the exact exploit payload
2. Confirming the exploit is blocked
3. Capturing before/after screenshots
4. Running SAST scan (6 → 0 findings)